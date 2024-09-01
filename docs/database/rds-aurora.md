# AWS RDS Aurora

MySQL and PostgreSQL-compatible relational database built for the cloud. Performance and availability of commercial-grade databases at 1/10th the cost.

## Configuration

Update the following environment variables to configure your RDS database.

```bash
# Set the region of the existing RDS Aurora MySQL
LESGO_AWS_RDS_AURORA_MYSQL_REGION=ap-southeast-1

# Set the name of the database it should connect to
LESGO_AWS_RDS_AURORA_MYSQL_DB_NAME=my_db

# Set the KMS Secret ID to connect to the RDS Proxy
LESGO_AWS_RDS_AURORA_MYSQL_PROXY_DB_CREDENTIALS_SECRET_ID=my_dbProxyCredentials
```

!!! info "AWS KMS to store Proxy Credentials"

    It is strongly recommended to store secret keys via AWS Key Management Service (AWS KMS). Ensure the following database credentials are supplied in the KMS Store: `host`, `username`, `password`.

!!! important "RDS Proxy"

    Lesgo! utilizes the RDS Proxy to manage connections to your database. This is important for serverless architecture to prevent each running instance from creating their own connection, eventually exhausting the databse connection limit.

## Resource Creation

Should you want Lesgo! Framework to create your RDS Aurora instance, ensure the following environment variables are set as well.

```bash
# Set the connection type. Proxy connection is recommended
LESGO_AWS_RDS_AURORA_MYSQL_CONNECTION_TYPE=proxy

# Set the minimum number of running instances to be available at any one time
LESGO_AWS_RDS_AURORA_MYSQL_SCALE_MIN_CAPACITY=1

# Set the maximum number of running instances to be available at any one time
LESGO_AWS_RDS_AURORA_MYSQL_SCALE_MAX_CAPACITY=1

# Set the deletion protection to prevent accidental termination and loosing your data
LESGO_AWS_RDS_AURORA_MYSQL_DELETION_PROTECTION=true
```

Be sure to include the relevant resource yml file in the `serverless.yml > resources` file. See `config/resources/sample-rdsProxy.yml` for sample resource creation.

## Database Connection

Connecting to the database is automatically handled when you execute your query. Database connection is managed by RDS Proxy.

## Terminating Database Connection

It is important to ensure your database connection is terminated once it is no longer required. By default, connections do not terminate and will remain idle. This is intended to reduce the resource needed when establishing a new connection. 

However, this is inefficient for serverless architecture. As such, it is important to terminate the connection once it is no longer required.

To terminate the RDS Proxy Connection, call the `disconnectDb` and attach it to the `disconnectMiddleware()` middleware.

See [disconnectMiddleware](../basics/middlewares/disconnectMiddleware.md) for usage.

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
import db from "Utils/db";

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
import db from "Utils/db";

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
import db from "Utils/db";

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
import db from "Utils/db";

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
import prepSQLInsertParams from "Utils/prepSQLInsertParams";
import validateFields from "Utils/validateFields";
import db from "Utils/db";

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
import db from "Utils/db";

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
import prepSQLUpdateParams from "Utils/prepSQLUpdateParams";
import validateFields from "Utils/validateFields";
import db from "Utils/db";

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
import db from "Utils/db";

const data = await db.query("SELECT * FROM users WHERE id = :id", {
  id: 1,
});
```
