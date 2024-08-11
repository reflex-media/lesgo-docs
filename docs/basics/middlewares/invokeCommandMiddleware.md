# Invoke Command Middleware

This middleware will be used for Invoke Command functions.

## Usage

```ts
import middy from '@middy/core';

interface MiddyInvokeCommandEvent {
  dropTableIfExists?: boolean;
}

const commandHandler = async (event: MiddyInvokeCommandEvent) => {
  const { dropTableIfExists } = event;

  return {
    dropTableIfExists
  }
};

export const handler = middy()
  .use(
    invokeCommandMiddleware({
      debugMode: appConfig.debug,
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
