# Object Store

Lesgo! is pre-configured with AWS S3 for object storage.

## Fetch Object

The `getObject()` function will fetch the object from the Bucket.

```js
import { getObject } from 'Utils/objectStore';

// Returns a buffered object response. See AWS for more information
const objectFile = await getObject('Key', 'Bucket');
```

## S3 Bucket Permissions Policy

To fetch/put objects to an exisitng S3 bucket, be sure to set up an IAM user with the correct permissions as well as updating the S3 bucket policy. You may override the config by updating these in the environment file.

```apache
# Set IAM access key with S3 access
AWS_S3_OPTIONS_ACCESS_KEY_ID=

# Set IAM secret key
AWS_S3_OPTIONS_SECRET_ACCESS_KEY=

# Set S3 region to connect to
AWS_S3_OPTIONS_REGION=
```
