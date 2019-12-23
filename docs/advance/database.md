# Database

Lesgo! allows you to connect to existing databases via Knex ORM or raw SQL.

## Configuration

The database configuration for your application is located at `src/config/database.js` for Knex ORM and `src/config/db.js` for raw SQL.

You may also simply update the respective environment files in `config/environments/*` as such:

```bash
DB_HOST=127.0.0.1
DB_HOST_WRITE=127.0.0.1
DB_HOST_READ=127.0.0.2
DB_PORT=3306
DB_USER="app_user"
DB_PASSWORD="app_password"
DB_NAME="app_db"
```

## Read & Write Connections

Sometimes you may wish to use one database connection for SELECT statements, and another for INSERT, UPDATE, and DELETE statements. Simply use the right Utils method for this.

Using read / write connection:

```js
import { db, dbRead } from "Utils/db";

// Fetch data from READ HOST
const dataRead = await dbRead.query("SELECT * FROM users LIMIT 1");
// Fetch data from WRITE HOST
const dataWrite = await db.query("SELECT * FROM users LIMIT 1");
```

## Running Raw SQL Queries

Once you have configured your database connection, you may run queries using the `Utils/db` module.

### Running A Select Query

To run a basic query, you may use the query method on the `Utils/db`:

```js
import { db, connectDb } from "Utils/db";

// Set up initial connection to database
connectDb();

// Fetch data
const data = await db.query("SELECT * FROM users LIMIT 1");
await db.end();

return data;
```

Refer to `src/handlers/samples/db.js` for usage example.

### Using Parameter Bindings

You may also use Parameter Bindings for your queries.

```js
import { db, connectDb } from "Utils/db";

// Set up initial connection to database
connectDb();

// Fetch data
const data = await db.query("SELECT * FROM users WHERE id=? LIMIT 1", [1]);
await db.end();

return data;
```

### Ending Connection

Though not necessary, it is recommended to always end the connection once you are done with it by simply calling the `end()` method.

## Using Knex ORM

### Defining Models

...

### Defining Relationships

...

### Retrieving Models

...
