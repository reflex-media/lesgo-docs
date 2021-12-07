# Error Handling

It is recommended to always throw an Error class as an exception instead of returning just an error message. You may create your own Error Class within the `src/exceptions/` directory.

```js
import MediaException from 'Exceptions/MediaException';
...

try {
  return validateFields(params, validFields);
} catch (err) {
  throw new MediaException(
    err.message,
    `Core/medias/getMedia::FIELD_VALIDATION_EXCEPTION`,
    400,
    { params, err }
  );
}
```

## Custom Error Classes

You can define your own Error Classes as required. Refer to the existing `src/exceptions/ErrorException.js` class.

## Sentry

Lesgo! is pre-configured to work with sentry out of the box

### Configuration

The sentry configuration for your application is located at `src/config/sentry.js`.

You may also simply update the respective environment files in `config/environments/*` as such:

```bash
# Whether to enable sentry or not
SENTRY_ENABLED="true"

# Where to send events
SENTRY_DSN="https://public@sentry.example.com/1"

# Minimal level for error reporting. Ref: https://github.com/winstonjs/winston#logging
SENTRY_LEVEL="error"

# Track release version and for sourcemaps reference
SENTRY_RELEASE="sls-my-project-maste"
```

### Initialization

Call this as early as possible on any of your lambdas. A suggestion would be to put this inside of a middleware

```js
import connectSentry from "Utils/sentry";

connectSentry();
```
