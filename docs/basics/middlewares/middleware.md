# Middleware

Middlewares can be executed before or after a request, usually handled by the handlers. This will be useful for cases where an action is required prior to reaching the handler, or when an action is required to execute prior to the returning of the response.

Middlewares require the [Middy NPM](https://www.npmjs.com/package/middy) package to work.

Middlewares should be written in the `src/middlewares/` directory.

## Available Middlewares

Lesgo! comes with a few pre-existing middlewares that you can use right away.

You may also import other ready-made middlewares from the [Middy repository](https://www.npmjs.com/package/middy#available-middlewares).

- [HttpMiddleware](/httpMiddleware.md)
- [SQSMiddleware](/sqsMiddleware.md)

## Nesting Middlewares

Middlewares can be nested together to achieve the desired results.

For example, the **HttpMiddleware** consists of the following middlewares:

- [@middy/do-not-wait-for-empty-event-loop](https://middy.js.org/docs/middlewares/do-not-wait-for-empty-event-loop)
- [@middy/http-event-normalizer](https://middy.js.org/docs/middlewares/http-event-normalizer)
- [@middy/http-header-normalizer](https://middy.js.org/docs/middlewares/http-header-normalizer)
- [@middy/http-json-body-parser](https://middy.js.org/docs/middlewares/http-json-body-parser)
- lesgo/middlewares/disconnectOpenConnectionsMiddleware
- lesgo/middlewares/httpResponseMiddleware

### Verify JWT Middleware

This middleware will verify any JWT passed to the `Authorization` header of the http request. The decoded JWT can be accesed through `handler.event.jwt` once successfully verified.

**Configuration**

The following JWT environment variables must be added to the respective environment files.

```apache
# Comma-delimited secret keys. 
# If kid is being used, separate them with ":" i.e.; kid1:secret1,kid2:secret2
LESGO_JWT_SECRET_KEYS=

# JWT algorithm used to sign / verify the token
LESGO_JWT_ALGORITHM=HS256

# Time to expire upon creation
LESGO_JWT_EXPIRESIN=1h

# Issuer claim
LESGO_JWT_ISSUER=lesgo-dev

# Audience claim
LESGO_JWT_AUDIENCE=lesgo-dev

# Set to true to verify claims.
LESGO_JWT_VALIDATE_CLAIMS=true
```

**Usage**

```js
import middy from '@middy/core';
import verifyJwtTokenMiddleware from "Middlewares/verifyJwtTokenMiddleware";

const originalHandler = event => {
  return event.collection;
};

export const handler = middy(originalHandler);

handler.use(verifyJwtTokenMiddleware());
```

## Custom Middlewares

You can write your own custom middleware with [Middy](https://www.npmjs.com/package/middy#writing-a-middleware).
