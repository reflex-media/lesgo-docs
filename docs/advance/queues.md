# Queues

Lesgo! is pre-configured with AWS SQS for queues.

## Queue Dispatch Event

The `Utils/dispatch` function will dispatch a message payload to the queue.

### Usage

This example usage will send a message to the pre-defined PingQueue.

```js
import { dispatch } from "Utils/queue";

const payload = {
  someData: "someValue",
};

return await dispatch(payload, "pingQueue");
```

## Consuming Dispatached Queued Messages

Messages sent to SQS can automatically trigger and be executed by a Lambda Function. This can be configured to an event lambda function within the `config/functions`.

> `config/functions/dequeue.yml`
```yaml
dequeue:
  handler: ${self:custom.path.app}/handlers/dequeue.handler
  description: Receives message from SQS for actual business logic execution
  timeout: 15
  events:
    - sqs: arn:aws:sqs:${self:provider.region}:${env:AWS_ACCOUNT_ID}:${self:provider.stackName}-dequeue
```

## Batch Processing Queued Messages

Messages can also be fetched and executed in batches.

...

## Processing Queued Messages with FIFO

Messages can also be consumed in FIFO.

...