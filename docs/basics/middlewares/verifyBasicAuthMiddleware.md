# VerifyBasicAuthMiddleware

This middleware reads the Basic Auth header and attaches the username field to the AWS API Gateway Event, and should be used for HTTP endpoints where authorization is required.

The username will be attached to the APIGatewayProxyEvent.basicAuth field.

## Usage

```ts
import middy from '@middy/core';
import { APIGatewayProxyEvent } from 'aws-lambda';
import { httpMiddleware, verifyBasicAuthMiddleware } from 'lesgo/middlewares';
import appConfig from '../../config/app';

interface MiddyAPIGatewayProxyEvent extends APIGatewayProxyEvent {
  basicAuth: {
    username: string;
  };
}

const pingHandler = (event: MiddyAPIGatewayProxyEvent) => {
  const { basicAuth } = event;

  return {
    username: basicAuth.username,
  }
};

export const handler = middy()
  .use(httpMiddleware())
  .use(verifyBasicAuthMiddleware())
  .handler(pingHandler);

export default handler;
```

## Error Response

Possible error response

```json
{
    "status": "error",
    "data": null,
    "error": {
        "code": "lesgo.middlewares.verifyBasicAuthMiddleware::INVALID_BASIC_AUTH_MISSING_FIELDS",
        "message": "Invalid basic auth due to missing fields",
        "details": {
            "name": "LesgoException",
            "message": "Missing required 'username'",
            "code": "lesgo.utils.validateFields::MISSING_REQUIRED_USERNAME",
            "extra": {
                "field": {
                    "key": "username",
                    "type": "string",
                    "required": true
                }
            }
        }
    },
    "_meta": {}
}
```
