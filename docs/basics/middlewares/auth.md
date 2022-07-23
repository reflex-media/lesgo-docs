These middlewares can be utilized as a layer of authentication before going through your HTTP requests.

## Verify JWT

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

## Basic Auth

This middleware will verify any basic auth passed to the `Authorization` header of the http request. Throws `Middlewares/basicAuthMiddleware::AUTH_INVALID_CLIENT_OR_SECRET_KEY` when a basic auth cannot be found in the clients list configuration.

This middleware can be used together with `clientAuthMiddleware`, as long as `clientAuthMiddleware` is declared first.
This is to allow the middleware to identify the matched `handler.event.platform`.

**Configuration**

The basic auth configuration for your application is located at `src/config/client.js`. Or copy [this file](https://raw.githubusercontent.com/reflex-media/lesgo/master/src/config/client.js) to that path.

**Usage**

```js
import middy from '@middy/core';
import basicAuthMiddleware from "Middlewares/basicAuthMiddleware";

const originalHandler = event => {
  return event.collection;
};

export const handler = middy(originalHandler);

handler.use(basicAuthMiddleware());

// or

handler.use(basicAuthMiddleware({
    // This is set as an override for Config/client.js when needed
    client: {
        myApp: {
            key: 'myappkeystring',
            secret: 'myappsecretstring'
        }
    }
}));
```

## Client Auth

This middleware will verify key passed to `x-client-id` header of the http request and sets the matched key to `handler.event.platform`. Throws `Middlewares/clientAuthMiddleware::INVALID_CLIENT_ID` when a basic auth cannot be found in the clients list configuration.

**Configuration**

The basic auth configuration for your application is located at `src/config/client.js`. Or copy [this file](https://raw.githubusercontent.com/reflex-media/lesgo/master/src/config/client.js) to that path.

**Usage**

```js
import middy from '@middy/core';
import clientAuthMiddleware from "Middlewares/clientAuthMiddleware";

const originalHandler = event => {
  return event.collection;
};

export const handler = middy(originalHandler);

handler.use(clientAuthMiddleware());
```