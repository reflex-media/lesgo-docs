# Queues

Lesgo! uses AWS SQS for queue management.

## Configuration

Update the following environment variables to start using AWS SQS.

```bash
# Set the AWS SQS region. Remove if using the default AWS region
LESGO_AWS_SQS_REGION=

# Add the aliases for your SQS Queues.
LESGO_AWS_SQS_QUEUE_ALIASES=
```

## Setting up SQS

You may create new SQS Queues via serverless config.

Create a new `sqs.yml` file under the `config/resources/` directory.

```yaml
# Declaration of a new Resource module
Resources:
  # Alias of the Queue Name. This will be used as a reference name
  sqsEventQueue:
    # Define the type of the resource
    Type: AWS::SQS::Queue
    # Define the properties of this queue
    Properties:
      # Set the actual queue name
      QueueName: ${self:provider.stackName}-sqsEventQueue
      # Message will return back to queue after this timeout while in-flight
      VisibilityTimeout: 65 
      # Message will be removed after 24h in the queue
      MessageRetentionPeriod: 86400
      # Max wait time before pushing to the dequeue event
      ReceiveMessageWaitTimeSeconds: 20
      # Define the policy for failed queues
      RedrivePolicy:
        # Target the Dead Letter Queue to process failed messages in the Queue
        deadLetterTargetArn:
          Fn::GetAtt: [sqsEventDLQ, Arn]
        # Max number of retry attempts before pushing to DLQ
        maxReceiveCount: 1
```

##  DLQ

Setting up the Dead Letter Queue (DLQ) allows for failed attempts to be processed via a different function.

You may add the additional Queue within the same resource object,

```yaml
Resources:
  ...
  sqsEventDLQ:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: ${self:provider.stackName}-sqsEventDLQ
      # Max time message should remain be in the DLQ. Messages past this time will be deleted.
      MessageRetentionPeriod: 86400
      # Max wait time before pushing to the dequeue event
      ReceiveMessageWaitTimeSeconds: 20
```

## FIFO Queue

If not defined, all queues are of the Standard type. Standard type queues have no ordering and may be processed at any time.

To ensure queues are only processed one at a time and in the order that they arrive, set the type to FIFO.

```yaml
Resources:
  ...
  sqsEventQueue:
    Type: AWS::SQS::Queue
    Properties:
      ...
      # Set the 2 properties below for FIFO queue
      FifoQueue: true
      ContentBasedDeduplication: true
```

## Usage

There are 2 methods for processing messages in the queue.

### Long polling

SQS queues can be attached to a lambda function for automatic processing of messages. This will subscribe the lambda function to the SQS queue as a long polling event.

Create a new `message-processing.yml` file under the `config/functions/` directory.

```yaml
sampleSQSDequeue:
  handler: ${self:custom.path.app}/handlers/message-processing/dequeue.handler
  description: Test lambda receiving Queue event
  memorySize: 512
  timeout: 5
  reservedConcurrency: 1
  events:
    - sqs:
        arn:
          Fn::GetAtt: [sqsEventQueue, Arn]
        batchSize: 5
        maximumConcurrency: 5
```

- `batchSize` - Maximum number of messages in the queue that the Lambda function receives in a single batch from the SQS queue. The Lambda function will either wait for the `batchSize` or `ReceiveMessageWaitTimeSeconds`
- `maximumConcurrency` - Maximum Lambda functions can be invoked simultaneously to process messages from the queue

### Manual processing of messages

You may also process messages via manual lambda invocation. This is especially useful for FIFO queues or putting into a scheduled job.

```ts
import { deleteMessage, receiveMessages } from 'lesgo/utils/sqs';

const processMessages = async () => {
  const messagesFetched = await receiveMessages('httpEventQueue.fifo', {
    // Set the number of messages to fetch for this job
    MaxNumberOfMessages: 5,
    // Set the max time to wait before fetching the messages
    WaitTimeSeconds: 1,
  });

  const records = messagesFetched.Messages;

  await records!.reduce(async (promise, record) => {
    await promise;

    const messageBody = JSON.parse(record.Body!);
    
    // process the messageBody

    // Important to delete the message once processed
    await deleteMessage('httpEventQueue.fifo', record.ReceiptHandle!);
  }, Promise.resolve());
}

export default processMessages;
```

!!! info "Important to delete messages once processed successfully"

    It is important to delete the messages once it is processed successfully. Note that this is only required for manual fetching of messages. If you are using long polling, this is handled automatically. Not deleting the messages may result the messages continue to return back to the queue.

### Dispatch message to Queue

The example below will dispatch a message to the Queue.

```ts
import { dispatch } from 'lesgo/utils/queue';

const payload = {
  someData: 'someValue',
};

return await dispatch(payload, 'pingQueue');
```
