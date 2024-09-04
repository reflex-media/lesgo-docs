# Disconnect Middleware

This middleware disconnects any open connections to resources. Be sure to include the Clients to disconnect as needed.

## Usage

Currently Lesgo! Framework supports disconnect for AWS RDS Proxy and AWS ElastiCache Redis instances. You may create your own disconnect functions and attach them to the same `disconnectMiddleware()`.

### Disconnect RDS Aurora MySQL Proxy Client

```typescript
import { APIGatewayProxyEvent } from 'aws-lambda';
import { disconnectMiddleware, httpMiddleware } from 'lesgo/middlewares';
import { disconnectDb } from 'lesgo/utils/db/mysql/proxy';

const functionHandler = async (event: APIGatewayProxyEvent) => {
  // Some code logic
};

export const handler = middy()
  .use(
    // Attach the disconnectMiddleware
    disconnectMiddleware({
      // List the Clients to disconnect
      clients: [disconnectDb],
    })
  )
  .use(httpMiddleware())
  .handler(functionHandler);

export default handler;
```

### Disconnect ElastiCache Redis Client

```typescript
...
```