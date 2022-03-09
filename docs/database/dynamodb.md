# AWS DynamoDB

Fast NoSQL key-value database.

## Configuration

Create a new YAML configuration file `dynamodb.yml` and place it under `config/resources/` directory.

```yaml
# config/resources/dynamodb.yml

Resources:
  settingsTable:
    Type: AWS::DynamoDB::Table
    DeletionPolicy: Retain
    Properties:
      TableName: ActorsTable
      AttributeDefinitions:
        - AttributeName: actorName
          AttributeType: S
        - AttributeName: movieName
          AttributeType: S
      KeySchema:
        - AttributeName: actorName
          KeyType: HASH
        - AttributeName: movieName
          KeyType: RANGE
      BillingMode: PAY_PER_REQUEST
```

Add the YAML file link to `serverless.yml` under the `resources` section.

```yaml
# serverless.yml

resources:
  - config/resources/dynamodb.yml
```

## Running DynamoDB Queries

### Retrieving Records

`dynamodb.query()` will return all matching records based on pre-determined primary key, primary and sort key combination, or global secondary index key.

```js
dynamodb.query(
  tableName: String,                 // name of the dynamodb table
  keyConditionExpression: String,    // primary or primary with sort or GSI key condition
  expressionAttributeValues: Object, // key-value pair
  projectionExpression: String = ""  // data to return
);
```

**Usage**

```js
import dynamodb from "Utils/dynamodb";

const user = await dynamodb.query(
  "ActorsTable",
  "actorName = :actorName and movieName = :movieName",
  {
    ":actorName": "Tom Hanks",
    ":movieName": "Toy Story",
  }
);

/**
[
  {
    actorName: 'Tom Hanks',
    movieName: 'Toy Story',
  },
  ...
] */
```

### Retrieving Record Count

`dynamodb.queryCount()` will return the number of query records.

```js
dynamodb.queryCount(
  tableName: String,                 // name of the dynamodb table
  keyConditionExpression: String,    // primary or primary with sort or GSI key condition
  expressionAttributeValues: Object, // key-value pair
);
```

**Usage**

```js
import dynamodb from "Utils/dynamodb";

const count = await dynamodb.queryCount(
  "ActorsTable",
  "actorName = :actorName",
  {
    ":actorName": "Tom Hanks",
  }
);

// 5
```

### Inserting a Record

`dynamodb.put()` will insert a new record.

```js
dynamodb.put(
  tableName: String,
  item: Object
});
```

**Usage**

```js
import dynamodb from "Utils/dynamodb";

const data = await dynamodb.put("ActorTable", {
  actorName: "Keanue Reaves",
  movieName: "Speed",
});
```

### Updating Existing Record

`dynamodb.update()` will update an existing record.

```js
dynamodb.update(
  tableName: String,
  key: Object,
  updateExpression: String,
  expressionAttributeValues: Object,
});
```

**Usage**

```js
import dynamodb from "Utils/dynamodb";

const data = await dynamodb.update(
  "ActorsTable",
  {
    actorName: "Tom Hanks",
    movieName: "Toy Story",
  },
  "set role = :role",
  {
    ":role": "Woody",
  }
);
```

### Native Client

Should you need to execute any other methods not already available, use the exported `dynamodb.client` to execute any of the available client functions.

**Example available client query method**

```js
const resp = await dynamodb.client
  .query({
    ExpressionAttributeValues: {
      ":actorName": "Tom Hanks",
    },
    KeyConditionExpression: "actorName = :actorName",
    ProjectionExpression: "actorName, movieName",
    TableName: "ActorTable",
  })
  .promise();
```

Refer to the official documentation [here](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html).
