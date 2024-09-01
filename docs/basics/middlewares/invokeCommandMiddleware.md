# Invoke Command Middleware

This middleware will be used for Invoke Command functions.

## Usage

```typescript
import middy from '@middy/core';
import { APIGatewayProxyEvent } from 'aws-lambda';

const commandHandler = async (event: APIGatewayProxyEvent) => {
  // Some code logic
};

export const handler = middy()
  .use(
    invokeCommandMiddleware({
      debugMode: false,
    })
  )
  .handler(commandHandler);

export default handler;
```

## Nested Middlewares

The following middlewares are used within this middleware.

- [@middy/do-not-wait-for-empty-event-loop](https://middy.js.org/docs/middlewares/do-not-wait-for-empty-event-loop)
- [@middy/http-event-normalizer](https://middy.js.org/docs/middlewares/http-event-normalizer)
- lesgo/middlewares/disconnectOpenConnectionsMiddleware
