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

## RDS Proxy

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

To terminate the RDS Proxy Connection, call the `disconnectDb()` and attach it to the `disconnectMiddleware()` middleware.

See [disconnectMiddleware](../basics/middlewares/disconnectMiddleware.md) for usage.

## Sample Database Queries

### Retrieving all records

```typescript
import { query } from 'lesgo/utils/db/mysql/proxy';

type Movie = {
  id: number;
  name: string;
}

const tableName = 'movies';
const sql = `SELECT * FROM ${tableName} ORDER BY id DESC`;

const resp = (await query(sql)) as Movie[] | [];
return resp;

/**
 * [
 *   {
 *     "id": 1,
 *     "name": "Spider-Man"
 *   }
 * ]
 */
```

### Retrieving a single record

```typescript
import { query } from 'lesgo/utils/db/mysql/proxy';

type Movie = {
  id: number;
  name: string;
}

const tableName = 'movies';
const sql = `SELECT * FROM ${tableName} WHERE id = ?`;
const movieId = 5;
const queryParams = [movieId]

const resp = (await query(sql, queryParams)) as Movie[] | [];
return resp[0];

/**
 * {
 *   "id": 5,
 *   "name": "Deadpool"
 * }
 */
```

### Inserting record

```typescript
import { query } from 'lesgo/utils/db/mysql/proxy';
import { getCurrentTimestamp, formatUnixTimestamp } from 'lesgo/utils';

const tableName = 'movies';
const sql = `INSERT INTO ${tableName} (name, createdAt, updatedAt, deletedAt) VALUES (?, ?, ?, ?)`;

const dateTimeNow = formatUnixTimestamp(getCurrentTimestamp());
const data = {
  name: 'Ironman',
  createdAt: dateTimeNow,
  updatedAt: dateTimeNow,
  deletedAt: null,
};
const queryParams = [
  data.name,
  data.createdAt,
  data.updatedAt,
  data.deletedAt
]

await query(sql, queryParams);
```

### Updating record

```typescript
import { query } from 'lesgo/utils/db/mysql/proxy';
import { getCurrentTimestamp, formatUnixTimestamp, isEmpty } from 'lesgo/utils';

const dataToUpdate = {
  name: 'Ironman 2'
}

const dateTimeNow = formatUnixTimestamp(getCurrentTimestamp());

const sqlSet: string[] = [];
const preparedValues: UpdateMovieModelInputKey[] = [];
Object.keys(dataToUpdate).forEach(key => {
  if (!isEmpty(params[key])) {
    sqlSet.push(`${key} = ?`);
    preparedValues.push(params[key]);
  }
});

sqlSet.push(`updatedAt = ?`);
preparedValues.push(dateTimeNow);

preparedValues.push(id);

const tableName = 'movies';
const sql = `UPDATE ${tableName} SET ${sqlSet.join(',')} WHERE id = ?`;

await query(sql, preparedValues);
```
