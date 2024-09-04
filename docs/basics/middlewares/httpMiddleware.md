# HttpMiddleware

This middleware normalizes all HTTP requests, handles, and formats success and error responses, and should be used for all HTTP endpoints.

## Usage

```ts
import middy from '@middy/core';
import { APIGatewayProxyEvent } from 'aws-lambda';
import { httpMiddleware } from 'lesgo/middlewares';
import appConfig from '../../config/app';

interface MiddyAPIGatewayProxyEvent extends APIGatewayProxyEvent {
  pathParameters: {
    'path-key-1': string,
    'path-key-2': string,
  },
  queryStringParameters: {
    stringValue: string;
    numberValue: number,
    booleanValue: boolean,
  },
  body: {
    user: {
      name: string;
      age: number;
    }
  };
}

const pingHandler = (event: MiddyAPIGatewayProxyEvent) => {
  const { pathParamters, queryStringParameters, body } = event;

  return {
    pathParamters,
    queryStringParameters, 
    body, 
  }
};

export const handler = middy()
  .use(httpMiddleware({ debugMode: appConfig.debug }))
  .handler(pingHandler);

export default handler;
```

## Success Response

The successful response will be formatted in this way

```json
{
    "status": "success",
    "data": {
        "pathParameters": {
            "path-key-1": "pathValue1",
            "path-key-2": "pathValue2",
        },
        "queryStringParameters": {
            "stringValue": "some-string",
            "numberValue": 999,
            "booleanValue": true,
        },
        "body": {
            "user": {
                "name": "John Doe",
                "age": 24
            }
        }
    },
    "_meta": {}
}
```

## Error Response

The error response will be formatted in this way

```json
{
    "status": "error",
    "data": null,
    "error": {
        "code": "handlers.auth.login::USER_NOT_EXIST",
        "message": "User does not exist",
        "details": {
            "name": "UserException",
            "message": "User not found",
            "statusCode": 404,
            "code": "models.User.loginUser::USER_NOT_EXIST",
            "extra": {}
        }
    },
    "_meta": {}
}
```

## Nested Middlewares

- [@middy/do-not-wait-for-empty-event-loop](https://middy.js.org/docs/middlewares/do-not-wait-for-empty-event-loop)
- [@middy/http-event-normalizer](https://middy.js.org/docs/middlewares/http-event-normalizer)
- [@middy/http-header-normalizer](https://middy.js.org/docs/middlewares/http-header-normalizer)
- [@middy/http-json-body-parser](https://middy.js.org/docs/middlewares/http-json-body-parser)
- [lesgo/middlewares/disconnectMiddleware](../middlewares/disconnectMiddleware.md)
- [lesgo/middlewares/httpResponseMiddleware](../middlewares/httpResponseMiddleware.md)
