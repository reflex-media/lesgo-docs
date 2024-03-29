# Middleware

Middlewares can be executed before or after a request, usually handled by the handlers. This will be useful for cases where an action is required prior to reaching the handler, or when an action is required to execute prior to the returning of the response.

Middlewares require [Middy npm](https://www.npmjs.com/package/middy) to work.

Middlewares should be written in the `src/middlewares/` directory.

## Available Middlewares

Lesgo! comes with 2 pre-existing middlewares.

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

## Custom Middlewares

You can write your own custom middleware with [Middy](https://www.npmjs.com/package/middy#writing-a-middleware).
