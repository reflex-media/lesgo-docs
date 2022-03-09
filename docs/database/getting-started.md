# Getting Started

As of today, Lesgo! supports the following database services:

- AWS DynamoDB
- AWS RDS Aurora Serverless
- AWS RDS Aurora Provisioned

**AWS DynamoDB**

[AWS DynamoDB](https://aws.amazon.com/dynamodb/) is highly recommended as it works seamlessly with Lambda, is on-demand, and considered a true serverless database; paid on demand per usage basis.

[See docs](dynamodb.md).

**AWS RDS Aurora Serverless**

Should you needed a traditional relational database, then [AWS RDS Aurora Serverless](https://aws.amazon.com/rds/aurora/serverless/) is the next best choice. Aurora Serverless is auto-scalable and pay only when it is up and running. It also allows you to connect to its Data API; removing the need to manage connection pools[^1] and leaving it to the [Data API](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/data-api.html) to manage that for you.

[See docs](rds-aurora.md#aurora-serverless).

**AWS RDS Aurora Provisioned**

Should you need to connect to an existing database that is already setup, [AWS RDS Aurora Provisioned](https://aws.amazon.com/rds/aurora) is also supported. It is recommedned to connect Lesgo! via RDS Proxy to manage connection pooling[^1].

[See docs](rds-aurora.md#aurora-provisioned).

[^1]: **Connection Pooling** is an important factor when considering for connecting to any database engine. This is because Lambda will need to establish a database connection before it can make any queries. As Lambda is scalable, each request will create its own connection, resulting in massive hits to the db connection load. With Data API and RDS Proxy, connection pooling is automatically handled for you with a separate service that is fully managed by AWS.
