# Database

As of today, Lesgo! only supports AWS DynamoDb and AWS Aurora Database Serverless, as these are the only true serverless database services available on AWS.

## RDS Aurora Serverless Database

### Configuration

The database configuration for your application is located at `src/config/db.js`.

You may also simply update the respective environment files in `config/environments/*` as such:

```bash
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

### Basic Queries

To run a basic query, you may use the query method on the `Utils/db`:

```js
import db from 'Utils/db';

return await db.query("SELECT * FROM users");
```

### Using Prepared Statement

You should always use prepared statement for your queries.

```js
import db from 'Utils/db';

return await db.query("SELECT * FROM users WHERE id = :id LIMIT 1", {
  id: 1,
});
```

### Additional methods

The `Utils/db` also comes with more specific and granular methods for specific queries.

#### db.select

This will return an array of Objects

```js
import db from 'Utils/db';

return await db.select("SELECT * FROM users WHERE is_deleted = :isDeleted", {
  isDeleted: false,
});
```

#### db.selectFirst

This will return only a single record

```js
import db from 'Utils/db';

return await db.selectFirst("SELECT * FROM users WHERE id = :id", {
  id: 1,
});
```

#### db.insert

This will return only the newly inserted primary key

```js
import db from 'Utils/db';

return await db.insert("INSERT INTO users(username,email,created_at) VALUES (:username, :email:, now())", {
  username: 'John',
  email: 'john@mail.com',
});
```

## AWS DynamoDb

If you'd prefer a non-relational database, DynamoDb is also supported.

### Configuration

Before you can start to use DynamoDb, you need to set it up under the `config/resources/` directory. Also include the newly created yml file in the `serverless.yml` under the `resources` section.

> `config/resources/dynamodb.yml`

```yml
Resources:
  settingsTable:
    Type: AWS::DynamoDB::Table
    DeletionPolicy: Retain
    Properties:
      TableName: ${self:provider.stackName}-settings
      AttributeDefinitions:
        - AttributeName: siteId
          AttributeType: S
        - AttributeName: checkType
          AttributeType: S
      KeySchema:
        - AttributeName: siteId
          KeyType: HASH
        - AttributeName: checkType
          KeyType: RANGE
      BillingMode: PAY_PER_REQUEST
```

> `serverless.yml`

```yml
resources:
  - ${file(${self:custom.path.resources}/dynamodb.yml)}
```

### Basic Queries

