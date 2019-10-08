# Task Scheduling

Lesgo! utilizes the CloudWatch Scheduled Event type to schedule events. This is the equivalent as you would configured a Cron entry on an application server.

## Defining Schedules

Define a scheduled event by first inserting a function with the `event:schedule` type in your serverless function yaml.

```yml
functions:
    - pingScheduledEvent:
        handler: src/app/handlers/scheduledEvent.handler
        description: Lambda event triggered from Scheduled Cloudwatch Event
        events:
            - schedule:
                name: scheduledEvent
                description: Trigger lambda function every 5 minute
                rate: rate(5 minutes)
                enabled: true
```

The above configuration will execute a CloudWatch Scheduled Event every 5 mins. This scheduled event will execute the handler.

Refer to `config/functions/ping.yml` for usage sample.
