# Middleware

Middlewares can be executed before or after a request, usually handled by the handlers. This will be useful for cases where an action is required prior to reaching the handler, or when an action is required to execute prior to the returning of the response.

Middlewares require the [Middy NPM](https://www.npmjs.com/package/middy) package to work.

Middlewares should be written in the `src/middlewares/` directory.

## Available Middlewares

Lesgo! comes with a few pre-existing middlewares that you can use right away.

You may also import other ready-made middlewares from the [Middy repository](https://www.npmjs.com/package/middy#available-middlewares).

- [HTTP Middleware](../middlewares//httpMiddleware.md)
- [HTTP Response Middleware](../middlewares/httpResponseMiddleware.md)
- [SQS Middleware](../middlewares/sqsMiddleware.md)
- [Invoke Command Middleware](../middlewares/invokeCommandMiddleware.md)
- [Disconnect Middleware](../middlewares/disconnectMiddleware.md)
- [Verify Basic Auth Middleware](../middlewares/verifyBasicAuthMiddleware.md)
- [Verify JWT Middleware](../middlewares/verifyJwtMiddleware.md)

## Nesting Middlewares

Middlewares can be nested together to achieve the desired results.

For example, the **HttpMiddleware** consists of the following middlewares:

- [@middy/do-not-wait-for-empty-event-loop](https://middy.js.org/docs/middlewares/do-not-wait-for-empty-event-loop)
- [@middy/http-event-normalizer](https://middy.js.org/docs/middlewares/http-event-normalizer)
- [@middy/http-header-normalizer](https://middy.js.org/docs/middlewares/http-header-normalizer)
- [@middy/http-json-body-parser](https://middy.js.org/docs/middlewares/http-json-body-parser)
- [lesgo/middlewares/disconnectMiddleware](../middlewares/disconnectMiddleware.md)
- [lesgo/middlewares/httpResponseMiddleware](../middlewares//httpResponseMiddleware.md)

## Custom Middlewares

You can write your own custom middleware with [Middy](https://www.npmjs.com/package/middy#writing-a-middleware).

## Middleware Ordering

Middy implements the classic onion-like middleware pattern, with some peculiar details.

The first `middleware.before()` will always be executed first with its `middleware.after()` being the last in that order.

See [https://middy.js.org/docs/intro/how-it-works/](https://middy.js.org/docs/intro/how-it-works/) for more info.
