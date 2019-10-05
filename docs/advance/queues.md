# Queues

Lesgo! is pre-configured with AWS SQS for queues.

## Dispatcher

The `dispatch()` function will dispatch a message payload to the queue.

### Usage

This example usage will send a message to the pre-defined PingQueue.

```js
import {dispatcher} from 'lesgo/utils';

const payload = {
  someData: 'someValue',
}

return await dispatch(payload, 'pingQueue');
```

Refer to `src/core/pingQueue.js` for usage example.

## Consumer

The consumer is a lambda function that will process messages payload in the Queue.

Refer to `src/core/pingQueueProcessor.js` for usage example.

### Automatic Consumption

Messages sent to the Queues can be automatically consumed by a Consumer. This can be configured to an event lambda function.

**Configuring automated queue processor**

...

### Scheduled Consumption

Messages sent to a FIFO Queue will need to be manually consumed. This can be configured as a CloudWatch Event.

**Configuring scheduled queue processor**

...

## Configuring Queue

Queues will need to be configured on Serverless via its yaml file before it can be used.

### Add Serverless Resource

## Connecting To Separate SQS Instance

To connect to a different SQS instance, you may override the config by updating these in the environment file.

```bash
# Set IAM access key with SQS access
AWS_SQS_OPTIONS_ACCESS_KEY_ID=

# Set IAM secret key
AWS_SQS_OPTIONS_SECRET_ACCESS_KEY=

# Set SQS region to connect to
AWS_SQS_OPTIONS_REGION=
```

## Sample Queue Endpoints

**`/ping/queue`**  
Send a ping request queued to SQS.  
**Note**: Set `x-api-key` in your request header for a valid request.

**`/ping/queue?failed-queue`**  
Send a ping request queued to SQS as a failed job.  
**Note**: Set `x-api-key` in your request header for a valid request.
