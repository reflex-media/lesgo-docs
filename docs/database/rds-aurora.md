# AWS RDS Aurora

MySQL and PostgreSQL-compatible relational database built for the cloud. Performance and availability of commercial-grade databases at 1/10th the cost.

## Configuration

The database configuration for your application is located at `src/config/db.js`. Or copy [this file](https://raw.githubusercontent.com/reflex-media/lesgo/master/src/config/db.js) to that path.

### Aurora Serverless

!!! note

    As Lesgo! establishes connection to Aurora Serverless via the Data API, prior setup and storing of the credentials on AWS Secret Manager is required. [Find out more](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/data-api.html).

For Aurora Serverless via Data API, create a `connections.dataApi` configuration as shown below.

```js
// src/config/db.js

export default {
  default: "dataApi",
  connections: {
    dataApi: {
      secretArn: process.env.DB_SECRET_ARN || "secretArnDataApi",
      secretCommandArn:
        process.env.DB_SECRET_COMMAND_ARN || "secretCommandArnDataApi",
      resourceArn: process.env.DB_RESOURCE_ARN || "resourceArnDataApi",
      database: process.env.DB_NAME || "databaseDataApi",
    },
  },
};
```

You may also simply update the respective environment files in `config/environments/.env.*` as such:

```apache
# AWS Secret Manager ARN to allow app db user connect to the specified db
DB_SECRET_ARN=""

# AWS Secret Manager ARN to allow app command db user connect to the specified db
# for running "command" like functions like database schema migration
DB_SECRET_COMMAND_ARN=""

# AWS Secret Manager ARN for the Aurora Serverless database cluster
DB_RESOURCE_ARN=""

# Database name to connect to
DB_NAME=""
```

### Aurora Provisioned

!!! note

    As Lesgo! establishes connection to Aurora Provisioned via the RDS Proxy, prior setup of the RDS Proxy is required. [Find out more](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/rds-proxy.html).

For Aurora Provisioned via RDS Proxy, create a `connections.rdsProxy` configuration as shown below.

```js
// src/config/db.js

export default {
  default: "rdsProxy",
  connections: {
    rdsProxy: {
      host:
        process.env.DB_RDS_PROXY_HOST ||
        "rds-cluster-proxy.proxy-ek9srsfbg2xc.us-west-2.rds.amazonaws.com",
      user: process.env.DB_RDS_PROXY_USER || "proxyUser",
      password: process.env.DB_RDS_PROXY_PASSWORD || "proxyPass",
      database: process.env.DB_NAME || "dbname",
    },
    rdsProxyRead: {
      host:
        process.env.DB_RDS_PROXY_HOST_READ ||
        process.env.DB_RDS_PROXY_HOST ||
        "rds-cluster-proxy-read-only.proxy-ek9srsfbg2xc.us-west-2.rds.amazonaws.com",
      user:
        process.env.DB_RDS_PROXY_USER_READ ||
        process.env.DB_RDS_PROXY_USER ||
        "proxyUser",
      password:
        process.env.DB_RDS_PROXY_PASSWORD_READ ||
        process.env.DB_RDS_PROXY_PASSWORD ||
        "proxyPass",
      database: process.env.DB_NAME || "dbname",
    },
  },
};
```

You may also simply update the respective environment files in `config/environments/env.*` as such:

```apache
# config/environments/.env.*

# Domain host of the RDS Proxy
DB_RDS_PROXY_HOST=""

# Domain host of the RDS Proxy for read-only connection
DB_RDS_PROXY_HOST_READ=""

# RDS Proxy username
DB_RDS_PROXY_USER=""

# RDS Proxy password
DB_RDS_PROXY_PASSWORD=""
```

## Database Connection

Connecting to the database is automatically handled when you execute your query.

For Aurora Serverless (via Data API), this is nothing to worry about as the opening and closing of database connections is handled within Data API itself! Thus, one less worry about managing connection pools.

For Aurora Provisioned (via RDS Proxy), a connection will be opened and closed per every query executed. While this works, it is not efficient as opening and closing a database connection will cost additional time (and money!).

As such, specifically for RDS Proxy, you should make use of Lesgo's persistent connection method `db.pConnect()`. This should be handled at the Handler level as much as possible.

### Persistent Connection for RDS Proxy

```js
// src/handlers/utils/ping.js

import middy from "@middy/core";
import httpMiddleware from "Lesgo/Middlewares/httpMiddleware";
import db from "Lesgo/Utils/db";
import ping from "Core/utils/ping";

const originalHandler = async (event) => {
  await db.pConnect();

  return ping(event.input);
};

export const handler = middy(originalHandler);

handler.use(httpMiddleware({ db }));
```

Notice the passing of the `db` object into `httpMiddleware()`. This is important to allow `httpMiddleware` to disconnect from RDS Proxy at the end of the lambda execution. Without doing so, you will encounter lambda timeout issues as the db connection is still open.

Should you want to handle the closing of the db connection yourself and without using any of Lesgo!'s middlewares, you can do so as shown below.

```js
// src/handlers/utils/ping.js

import middy from "@middy/core";
import db from "Lesgo/Utils/db";
import ping from "Core/utils/ping";

const originalHandler = async (event) => {
  await db.pConnect();

  try {
    const resp = await ping(event.input);
    return resp;
  } catch (err) {
    throw err;
  } finally {
    db.end();
  }
};

export const handler = middy(originalHandler);

handler.use();
```

The `finally` in `try catch` is important to allow for a safe disconnection of the db.

## Running Database Queries

### Retrieving All Rows

`db.select()` will return a promised array of objects.

```js
db.select(
  sql: String,
  sqlParams: Object,
  connectionOpts?: Object = {}
);
```

**Usage**

```js
import db from "Lesgo/Utils/db";

const data = await db.select(
  "SELECT * FROM users WHERE is_deleted = :isDeleted",
  {
    isDeleted: 0,
  }
);
```

### Retrieving a Single Row

`db.selectFirst()` will return a promised object of a single record.

```js
db.selectFirst(
  sql: String,
  sqlParams: Object,
  connectionOpts?: Object = {}
);
```

**Usage**

```js
import db from "Lesgo/Utils/db";

const data = await db.selectFirst("SELECT * FROM users WHERE id = :id", {
  id: 1,
});
```

### Retrieving Paginated Rows

`db.selectPaginate()` will return a promised object with pagination data and itemized rows.

```js
db.selectPaginate(
  sql: String,
  sqlParams: Object,
  perPage?: Number = 10,
  currentPage?: Number = 1,
  total?: Number = null,
  connectionOpts?: Object = {}
);
```

!!! note

    When `total` is not provided, `db.selectPaginate()` will run 2 separate queries to first fetch all the record count, followed by the actual query with `OFFSET` and `LIMIT`. It is strongly advisable to pass along the `total` for best performance.

**Usage**

```js
import db from "Lesgo/Utils/db";

const data = await db.selectPaginate(
  "SELECT * FROM users WHERE is_deleted = :isDeleted",
  {
    isDeleted: 0,
  },
  {
    perPage: 1,
    currentPage: 1,
    total: 25,
  }
);
```

### Inserting a Single Record

`db.insert()` will insert a new record and return only the newly inserted primary key.

```js
db.insert(
  sql: String,
  sqlParams: Object,
  connectionOpts?: Object = {}
);
```

**Usage**

```js
import db from "Lesgo/Utils/db";

const insertId = await db.insert(
  "INSERT INTO users(username,email) VALUES (:username, :email)",
  {
    username: "John",
    email: "john@mail.com",
  }
);
```

A much better approach to inserting records is to first validate the fields and then inserting it with `Utils/prepSQLInsertParams`.

```js
import prepSQLInsertParams from "Lesgo/Utils/prepSQLInsertParams";
import validateFields from "Lesgo/Utils/validateFields";
import db from "Lesgo/Utils/db";

const validFields = [
  { key: "username", type: "string", required: true },
  { key: "email", type: "string", required: true },
];

let validated = validateFields({ ...params }, validFields);

const { insertColumns, insertValues, insertFields } = prepSQLInsertParams(
  validated,
  validFields
);

await db.insert(
  `INSERT INTO users(${insertColumns}) VALUES(${insertValues})`,
  insertFields
);
```

Learn more about [Utils/validateFields](../advance/helpers.md#field-validator) and [Utils/prepSQLInsertParams](../advance/helpers.md#prep-insert-sql-parameter).

### Updating an Existing Record

`db.update()` will update an existing record and throw an Error if no record found for update.

```js
db.update(
  sql: String,
  sqlParams: Object,
  connectionOpts?: Object = {}
);
```

**Usage**

```js
import db from "Lesgo/Utils/db";

const insertId = await db.update(
  "UPDATE users SET username=:username, email=:email, updated_at=now()) WHERE id=:id",
  {
    id: 1,
    username: "John",
    email: "john@mail.com",
  }
);
```

As with insert, updatating an existing record is best done with `Utils/prepSQLUpdateParams`.

```js
import prepSQLUpdateParams from "Lesgo/Utils/prepSQLUpdateParams";
import validateFields from "Lesgo/Utils/validateFields";
import db from "Lesgo/Utils/db";

const validFields = [
  { key: "username", type: "string", required: true },
  { key: "email", type: "string", required: true },
];

const params = {
  username: "John",
  email: "john@mail.com",
};

let validated = validateFields(params, validFields);

const { updateColumnValues, wherePrimaryKey, updateFields } =
  prepSQLUpdateParams(validated, validFields);

await db.update(
  `UPDATE users SET ${updateColumnValues}, updated_at=NOW() WHERE ${wherePrimaryKey}`,
  updateFields
);
```

Learn more about [Utils/prepSQLUpdateParams](../advance/helpers.md#prep-update-sql-parameter).

### Raw Query

All of the above executes `db.query()`. You may also execute your queries directly and get a raw response.

```js
db.query(
  sql: String,
  sqlParams: Object,
  connectionOpts?: Object = {}
);
```

**Usage**

```js
import db from "Lesgo/Utils/db";

const data = await db.query("SELECT * FROM users WHERE id = :id", {
  id: 1,
});
```
