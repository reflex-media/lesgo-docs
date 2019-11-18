# Middleware

Middlewares can be executed before or after a request, usually handled by the handlers. This will be useful for cases where an action is required prior to reaching the handler, or when an action is required to execute prior to the returning of the response.

Middlewares require [Middy npm](https://www.npmjs.com/package/middy) to work.

Middlewares should be written in the `src/middlewares/` directory.

## Available Middlewares

Lesgo! comes with 6 pre-existing middlewares.

You may also import other ready-made middlewares from the [Middy repository](https://www.npmjs.com/package/middy#available-middlewares).

### Normalize Http Request

This middleware will normalize the query string parameters and/or json body in the request into a common `handler.event.input`. This middleware executes _before_ the handler is called.

**Usage**

```js
import middy from "middy";
import normalizeHttpRequest from "Middlewares/normalizeHttpRequest";

const originalHandler = event => {
  return event.input;
};

export const handler = middy(originalHandler);

handler.use(normalizeHttpRequest());
```

### Normalize SQS Message

This middleware will normalize records coming from sqs message event. The `Records` object in the `handler.event` will be normalized into `handler.event.collection`. This middleware executes _before_ the handler is called.

**Usage**

```js
import middy from "middy";
import normalizeSQSMessage from "Middlewares/normalizeSQSMessage";

const originalHandler = event => {
  return event.collection;
};

export const handler = middy(originalHandler);

handler.use(normalizeSQSMessage());
```

Refer to `src/handlers/samples/pingQueueProcessor.js` for more detailed usage example.

### Success Http Response

This middleware will be executed whenever a successful response is expected to be returned. This middleware executes _after_ the request is processed and _before_ the response is returned.

**Usage**

```js
import middy from "middy";
import successHttpResponse from "Middlewares/successHttpResponse";

const originalHandler = event => {
  return event.input;
};

export const handler = middy(originalHandler);

handler.use(successHttpResponse());
```

### Error Http Response

This middleware will be executed whenever an error response is expected to be returned. This middleware executes _after_ the request is processed and _before_ the response is returned.

**Usage**

```js
import middy from "middy";
import errorHttpResponse from "Middlewares/errorHttpResponse";

const originalHandler = event => {
  return event.input;
};

export const handler = middy(originalHandler);

handler.use(errorHttpResponse());
```

### Http

This middleware combines the `normalizeHttpRequest`, `successHttpResponse`, and `errorHttpResponse` middlewares, and can be used for all http endpoints (configured with API Gateway).

**Usage**

```js
import middy from "middy";
import httpMiddleware from "Middlewares/httpMiddleware";

const originalHandler = event => {
  return event.input;
};

export const handler = middy(originalHandler);

handler.use(httpMiddleware());
```

Refer to `src/handlers/ping.js` for usage.

## Custom Middlewares

You can write your own custom middleware with [Middy](https://www.npmjs.com/package/middy#writing-a-middleware).
