# Database

As of today, Lesgo! only supports AWS DynamoDb and AWS Aurora Database Serverless, as these are the only true serverless database services available on AWS.

## RDS Aurora Serverless Database

### Configuration

The database configuration for your application is located at `src/config/db.js`.

You may also simply update the respective environment files in `config/environments/*` as such:

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

### Basic Queries

To run a basic query, you may use the query method on the `Utils/db`:

```js
import db from "Utils/db";

const data = await db.query("SELECT * FROM users");

/**
[
  {
    id: 1,
    username: 'John Doe',
  },
  {
    id: 2,
    username: 'Jane Doe',
  },
  ...
]
*/
```

!!! warning "Warning"
    While it is OK to write raw queries with values inserted directly in the query statement, note that this is risky. Use prepared statement as show below as much as possible.

### Using Prepared Statement

You should always use prepared statement for your queries.

```js
import db from "Utils/db";

const data = db.query("SELECT * FROM users WHERE id = :id LIMIT 1", {
  id: 1,
});

/**
[
  {
    id: 1,
    username: 'John Doe',
  },
  {
    id: 2,
    username: 'Jane Doe',
  },
  ...
]
*/
```

### Available methods

The `Utils/db` also comes with more specific and granular methods for specific queries.

Refer to [data-api-client](https://www.npmjs.com/package/data-api-client) npm package for additioal details.

#### db.select

This will return an array of Objects

```js
import db from "Utils/db";

const darta = db.select("SELECT * FROM users WHERE is_deleted = :isDeleted", {
  isDeleted: false,
});

/**
[
  {
    id: 1,
    username: 'John Doe',
    is_deleted: false,
  },
  {
    id: 2,
    username: 'Jane Doe',
    is_deleted: false,
  },
  ...
]
*/
```

#### db.selectFirst

This will return only a single record

```js
import db from "Utils/db";

const data = db.selectFirst("SELECT * FROM users WHERE id = :id", {
  id: 1,
});

/**
{
  id: 1,
  username: 'John Doe',
}
*/
```

#### db.selectPaginate

This will return a paginated response

```js
import db from "Utils/db";

const paginatedData = await db.selectPaginate(
  "SELECT * FROM users WHERE id = :id",
  {
    id: 1,
  },
  {
    perPage: 3,
    currentPage: 1,
    total: 100,
  }
);

/**
{
  count: 25,
  previous_page: 1,
  current_page: 1,
  next_page: 2,
  last_page: 34,
  per_page: 5,
  total: 100,
  items: [
    {
      id: 1,
      username: 'John Doe',
    },
    {
      id: 2,
      username: 'Jane Doe',
    },
    {
      id: 3,
      username: 'Salmah Hayek',
    }
  ],
}
*/
```

!!! warning "Warning"
    If `total` is not provided, `db.selectPaginate` will execute the given query and calculate the total record count by calculating the length of the resulset with `.length`. It is highly recommended to provide the `total` to avoid this resource intensive process.

#### db.insert

This will insert a new record and return only the newly inserted primary key

```js
import db from "Utils/db";

const insertId = db.insert(
  "INSERT INTO users(username,email,created_at) VALUES (:username, :email:, now())",
  {
    username: "John",
    email: "john@mail.com",
  }
);

// 1
```

#### db.update

This will update an existing record. Will throw an Error if no record found for update

```js
import db from "Utils/db";

const insertId = db.update(
  "UPDATE users SET username=:username, email=:email, updated_at=now()) WHERE id=:id",
  {
    id: 1,
    username: "John",
    email: "john@mail.com",
  }
);
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

To run a basic query, you may use the query method on the `Utils/dynamodb`:

```js
import dynamodb from "Utils/dynamodb";

const data = await dynamodb.query(
  'stackName-settings',
  'siteId = :siteId',
  {
    ':siteId': 'default',
  },
  'siteId, checkType'
);

/**
[
  {
    siteId: 'site1',
    checkType: 'text',
  },
  {
    siteId: 'site2',
    checkType: 'photo',
  },
  ...
]
*/
```

### Using Custom Queries

You can also pass your own query if needed through the `client` property, refer to `query` method's [first parameter](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html#query-property).

```js
import dynamodb from "Utils/dynamodb";

const data = await dynamodb.client.query({
  ExpressionAttributeValues: {
    ':siteId': 'default',
    'siteId, checkType'
  },
  KeyConditionExpression: "siteId = :siteId", 
  ProjectionExpression: "siteId, checkType", 
  IndexName: 'settings-index',
  TableName: "stackName-settings"
 });

/**
{
  Items: [
    {
      siteId: 'site1',
      checkType: 'text',
    },
    {
      siteId: 'site2',
      checkType: 'photo',
    },
    ...
  ],
  Count: 5,
  ConsumedCapacity: {}
}

*/
```

### Available Methods

The `Utils/dynamodb` also comes with more specific and granular methods for specific queries.

#### dynamodb.queryCount

This will return the number of results

```js
import dynamodb from "Utils/dynamodb";

const count = await dynamodb.queryCount(
  'stackName-settings',
  'siteId = :siteId',
  {
    ':siteId': 'default',
  },
  'siteId, checkType'
);

// 5
```

#### dynamodb.put

This will insert a new record. Refer [here](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html#put-property) for response and more information.

```js
import dynamodb from "Utils/dynamodb";

const data = await dynamodb.put(
  'stackName-settings',
  {
    'siteId': 'default',
    'checkType': 'text'
  }
);
```

#### dynamodb.update

This will update an existing record. Refer [here](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html#update-property) for response and more information.

```js
import dynamodb from "Utils/dynamodb";

const data = await dynamodb.update(
  'stackName-settings',
  {
    'siteId': 'default',
    'checkType': 'text'
  },
  'set checkType = :checkType',
  {
    ':checkType': 'photo'
  }
);
```