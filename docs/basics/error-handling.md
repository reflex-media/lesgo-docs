# Error Handling

It is recommended to always throw an Error class as an exception instead of returning just an error message. You may create your own Error Class within the `src/exceptions/` directory.

```js
import ValidationError from 'Errors/ValidationError';

const someFunction = () => {
  throw new ValidationError(
    'Content type defined as JSON but an invalid JSON was provided',
    40001002,
    400
  );
};
```

Refer to the sample `src/handlers/samples/errorHandling.js` for usage.

To test the sample usage, uncomment `samples.yml` function in `serverless.yml` and deploy the application to a development environment.

Testable sample url endpoints:
- Test error message: `/samples/error-handling?type=message`.
- Test error exception: `/samples/error-handling?type=exception`.

## Custom Error Classes

You can define your own Error Classes as required. Refer to the existing `src/exceptions/ErrorException.js` class.
