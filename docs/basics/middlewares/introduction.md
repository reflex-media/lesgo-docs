# Middlewares

Middlewares can be executed before or after a request, usually handled by the handlers. This will be useful for cases where an action is required prior to reaching the handler, or when an action is required to execute prior to the returning of the response.

Middlewares require [Middy npm](https://www.npmjs.com/package/middy) to work.

Middlewares should be written in the `src/middlewares/` directory.

## Available Middlewares

Lesgo! comes with pre-existing middlewares.

- [HTTP Middlewares](/lesgo-docs/basics/middlewares/http)
- [Event Parsing Middlewares](/lesgo-docs/basics/middlewares/event-parsing)
- [Auth Middlewares](/lesgo-docs/basics/middlewares/auth)

You may also import other ready-made middlewares from the [Middy repository](https://www.npmjs.com/package/middy#available-middlewares).

## Custom Middlewares

You can write your own custom middleware with [Middy](https://www.npmjs.com/package/middy#writing-a-middleware).
