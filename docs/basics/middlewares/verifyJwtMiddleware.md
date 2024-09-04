# Verify JWT Middleware

This middleware reads the `Authorization` header and attaches the decoded JWT to the AWS API Gateway Event, and should be used for HTTP endpoints where authorization is required.

The decoded JWT will be attached to the `APIGatewayProxyEvent.jwt` field.

## Configuration

The following JWT environment variables must be added to the respective environment files.

```bash
# Comma-delimited secret keys. If kid is being used, separate them with ":" i.e.; kid1:secret1,kid2:secret2
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

## Usage

```typescript
import middy from '@middy/core';
import { APIGatewayProxyEvent } from 'aws-lambda';
import { httpMiddleware, verifyJwtMiddleware } from 'lesgo/middlewares';

interface MiddyAPIGatewayProxyEvent extends APIGatewayProxyEvent {
  jwt: string | Jwt | JwtPayload;
}

const pingHandler = (event: MiddyAPIGatewayProxyEvent) => {
  const { jwt } = event;

  return {
    jwt,
  }
};

export const handler = middy()
  .use(httpMiddleware())
  .use(verifyJwtMiddleware())
  .handler(pingHandler);

export default handler;
```

## Error Response

Possible error responses

```json
{
    "status": "error",
    "data": null,
    "error": {
        "code": "lesgo.middlewares.verifyJwtMiddleware::ERROR_VERIFYING_JWT",
        "message": "Error verifying JWT",
        "details": {
            "name": "LesgoException",
            "message": "kid invalid-kid not found.",
            "code": "lesgo.services.JWTService.getJwtSecret::KID_NOT_FOUND"
        }
    },
    "_meta": {}
}
```
