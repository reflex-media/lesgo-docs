# Middleware

Middlewares can be executed before or after a request, usually handled by the handlers. This will be useful for cases where an action is required prior to reaching the handler, or when an action is required to execute prior to the returning of the response.

Middlewares require [Middy npm](https://www.npmjs.com/package/middy) to work.

Middlewares should be written in the `src/middlewares/` directory.

## Available Middlewares

Lesgo! comes with 3 pre-existing middlewares.

You may also import other ready-made middlewares from the [Middy repository](https://www.npmjs.com/package/middy#available-middlewares).

### Http

This middleware normalizes all HTTP requests, handles, and formats success and error responses, and should be used for all HTTP endpoints. Will also provide any or both JSON body and url querystring parameters into a single `event.input`. This middleware will also populate with `event.auth.sub` when JWT is used and presented with the `Authorization` header.

**Usage**

```js
import middy from '@middy/core';
import httpMiddleware from "Middlewares/httpMiddleware";

const originalHandler = event => {
  return event.input;
};

export const handler = middy(originalHandler);

handler.use(httpMiddleware());
```

#### Success Response

The successfuly response will be formatted in this way
```json
{
    "status": "success",
    "data": {},
    "_meta": {}
}
```

#### Error Response

The successfuly response will be formatted in this way
```json
{
    "status": "error",
    "data": null,
    "error": {
        "code": "Core/users/getUser::USER_NOT_EXIST",
        "message": "UserException: User does not exist",
        "details": {
            "err": {
                "name": "UserException",
                "message": "User not found",
                "statusCode": 404,
                "code": "Core/users/getUser::USER_NOT_EXIST",
                "extra": {}
            }
        }
    },
    "_meta": {}
}
```

### Normalize SQS Message

This middleware will normalize records coming from sqs message event. The `Records` object in the `handler.event` will be normalized into `handler.event.collection`. This middleware executes _before_ the handler is called.

**Usage**

```js
import middy from '@middy/core';
import normalizeSQSMessage from "Middlewares/normalizeSQSMessage";

const originalHandler = event => {
  return event.collection;
};

export const handler = middy(originalHandler);

handler.use(normalizeSQSMessage());
```

### Verify JWT

This middleware will verify any JWT passed to the `Authorization` header of the http request. The decoded JWT can be accesed through `handler.event.decodedJwt`. If the JWT is verified, `handler.event.auth.sub` is set to the JWT's sub, else a `403` response will be thrown.

**Configuration**

The JWT configuration for your application is located at `src/config/jwt.js`. Or copy [this file](https://raw.githubusercontent.com/reflex-media/lesgo/master/src/config/jwt.js) to that path.

You may also simply update the respective environment files in `config/environments/*` as such:

```apache
# SHA256 JWT secret key, used to verify the token passed to "Authorization" header
JWT_SECRET=""

# Leave empty if you don't want the issuer to be validated
JWT_ISS_SHOULD_VALIDATE=

# Comma-separated list of domains to validate
JWT_ISS_DOMAINS=""

# Leave empty if you don't want the custom claims to be validated
JWT_CUSTOM_CLAIMS_SHOULD_VALIDATE=

# List of custom claims to valdiate.
# Visit https://auth0.com/docs/tokens/jwt-claims for more info
JWT_CUSTOM_CLAIMS_DATA=""
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
