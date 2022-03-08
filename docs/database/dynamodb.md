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
      TableName: ${self:provider.stackName}-<tableName>
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

Add the YAML file link to `serverless.yml` under the `resources` section.

```yaml
# serverless.yml

resources:
  - config/resources/dynamodb.yml
```