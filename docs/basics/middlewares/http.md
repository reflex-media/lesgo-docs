These middlewares can be utilized to parse HTTP requests and output HTTP in different specific formats.

## Http

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

## Http No Output

This middleware normalizes all HTTP requests, handles, and formats success and error responses to make sure it always returns a success 200 with no body.

**Usage**

```js
import middy from '@middy/core';
import httpNoOutputMiddleware from "Middlewares/httpNoOutputMiddleware";

const originalHandler = event => {
  return event.input;
};

export const handler = middy(originalHandler);

handler.use(httpNoOutputMiddleware());
```