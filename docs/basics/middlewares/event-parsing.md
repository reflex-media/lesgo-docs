These middlewares can be utilized for parsing data from specific services such as AWS Services, and just instead directly accessing it from a single list of recors inside `handler.event.collection`.

## Normalize SQS Message

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