# HttpResponseMiddleware

This middleware formats success and error responses. This is already nested within HttpMiddleware.

## Usage

See [HttpMiddleware](../middlewares/httpMiddleware.md) for usage info.

## Extended Response Data

Should you need to included additional response data beyond the standard structure, you may do so by suppling the data to `request.event.extendedResponse`.
This is particularly useful for cases like ensuring the authenticated user data is available for all authenticated requests.

```typescript
import middy from "@middy/core";

const verifyAccessMiddleware = () => {
  const verifyAccessMiddlewareBefore = async (request: middy.Request) => {
    // Validate access

    const authUser = {
      id: 1,
      username: "John_Doe",
    };

    request.event = {
      authUser,
    };
  };

  const verifyAccessMiddlewareAfter = async (request: middy.Request) => {
    request.event = {
      extendedResponse: {
        _authUser: authUser,
      },
    };
  };

  return {
    before: verifyAccessMiddlewareBefore,
    after: verifyAccessMiddlewareAfter,
  };
};

export default verifyAccessMiddleware;
```

### Sample response with extendedResponst

```json
{
    "status": "success",
    "data": {
      // some data
    },
    "_meta": {},
    "_authUser": {
      "id": 1,
      "username": "John_Doe"
    }
}
```
