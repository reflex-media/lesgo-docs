# Queues

Lesgo! is pre-configured with AWS SQS for queues.

## Configuration

The SQS configuration for your application is located at `src/config/aws.js`. Or copy [this file](https://raw.githubusercontent.com/reflex-media/lesgo/master/src/config/aws.js) to that path, a `sampleQueue` is created for you as a sample.

You may also simply update the respective environment files in `config/environments/*` as such:

```apache
# SQS Access Key ID
AWS_SQS_OPTIONS_ACCESS_KEY_ID=""

# SQS Secret Access Key
AWS_SQS_OPTIONS_SECRET_ACCESS_KEY=""

# SQS region name
AWS_SQS_OPTIONS_REGION=""
```

## Queue Dispatch Event

The `Utils/dispatch` function will dispatch a message payload to the queue.

### Usage

This example usage will send a message to the pre-defined PingQueue.

```js
import { dispatch } from "Lesgo/Utils/queue";

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