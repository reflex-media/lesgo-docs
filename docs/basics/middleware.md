# Middleware

Middlewares can be executed before or after a request, usually handled by the handlers. This will be useful for cases where an action is required prior to reaching the handler, or when an action is required to execute prior to the returning of the response.

Middlewares require the [Middy NPM](https://www.npmjs.com/package/middy) package to work.

Middlewares should be written in the `src/middlewares/` directory.

## Available Middlewares

Lesgo! comes with a few pre-existing middlewares that you can use right away.

You may also import other ready-made middlewares from the [Middy repository](https://www.npmjs.com/package/middy#available-middlewares).

### HttpMiddleware

This middleware normalizes all HTTP requests, handles, and formats success and error responses, and should be used for all HTTP endpoints.

**Usage**

```ts
import middy from '@middy/core';
import { APIGatewayProxyEvent } from 'aws-lambda';
import { httpMiddleware } from 'lesgo/middlewares';
import appConfig from '../../config/app';

type MiddyAPIGatewayProxyEvent = APIGatewayProxyEvent & {
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
};

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

#### Success Response

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

#### Error Response

The error response will be formatted in this way
```json
{
    "status": "error",
    "data": null,
    "error": {
        "code": "handlers.auth.login::USER_NOT_EXIST",
        "message": "UserException: User does not exist",
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

### SQS Middleware

This middleware will normalize records coming from sqs message event. This middleware executes _before_ the handler is called.

**Usage**

```ts
import middy from '@middy/core';
import { SQSEvent, SQSRecord } from 'aws-lambda';
import { sqsMiddleware } from 'lesgo/middlewares';

interface InsertRecordInput {
  userId: string;
  title: string;
}

type MiddySQSEventRecord = SQSRecord & {
  body: InsertRecordInput;
};

type MiddySQSEvent = SQSEvent & {
  Records: MiddySQSEventRecord[];
};

const dequeueHandler = async (event: MiddySQSEvent) => {
  const records = event.Records as MiddySQSEventRecord[];

  const processRecord = async (record: MiddySQSEventRecord) => {
    // Process the individual record
  };

  await Promise.all(records.map(record => processRecord(record)));
};

export const handler = middy()
  .use(sqsMiddleware())
  .handler(dequeueHandler);

export default handler;
```

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
