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
