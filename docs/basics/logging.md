# Logging

Lesgo! is configured with structured logging.

Structured logs will appear on the console by default.

```js
import { logger } from 'lesgo/utils';

logger.log('info', 'this is an info log');
logger.info('This is an info log');
logger.warn('This is a warning log');
logger.error('This is an error log');
```

Error logs can also be sent to Sentry. Simply update the relevant config in the `/config/environments/` directory.

```bash
# Enable/disable sentey reporting
SENTRY_ENABLED=true

# DSN for sentry reporting. Instructions can be found on your Sentry dashboard
SENTRY_DSN=

# Minimal error to send to Sentry.
SENTRY_LEVEL=
```

You may also add additional custom metadata as such:

```js
logger.info('This is an info log with my own custom metadata', {
  customData1: 'someData1',
  customData2: 'someData2',
});
```
