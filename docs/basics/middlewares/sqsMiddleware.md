# SQS Middleware

This middleware will normalize records coming from SQS message event. This middleware executes _before_ the handler is called.

## Usage

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

## Nested Middlewares

- [@middy/do-not-wait-for-empty-event-loop](https://middy.js.org/docs/middlewares/do-not-wait-for-empty-event-loop)
- [@middy/http-event-normalizer](https://middy.js.org/docs/middlewares/http-event-normalizer)
- [lesgo/middlewares/disconnectMiddleware](../middlewares/disconnectMiddleware.md)
