# Error Handling

It is recommended to always throw an Error class as an exception instead of returning just an error message. You may create your own Error Class within the `src/exceptions/` directory.

```typescript
import { MediaException } from 'src/exceptions';

const params = {
  someKey: 'someValue',
}

const validFields = [
  {
    key: 'someKey',
    type: 'string',
    required: true,
  }
]

try {
  return validateFields(params, validFields);
} catch (error: unknown) {
  throw new MediaException(
    'Invalid fields',
    `core.medias.getMedia::FIELD_VALIDATION_EXCEPTION`,
    400,
    { error, params }
  );
}
```

## Custom Error classes

You can define your own Error classes as needed. Refer to the existing `src/exceptions/ErrorException.ts` class.
