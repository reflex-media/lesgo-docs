# Object Store

Lesgo! is pre-configured with AWS S3 for object storage.

## Configuration

Update the following environment variables

```bash
# Set the AWS S3 region. Remove if using the default AWS region
LESGO_AWS_S3_REGION=

# Set the AWS S3 bucket to connect to
LESGO_AWS_S3_BUCKET=

# Set the public url for the AWS S3 object path
LESGO_AWS_S3_BUCKET_URI=
```

## Permissions

Access to the S3 bucket will be done via the IAM lambda role. Set the following permission to be able to get and put objects to the specific bucket.

```yml
provider:
  ...
  iamRoleStatements:
    - Effect: 'Allow'
      Action:
        - 's3:GetObject'
        - 's3:PutObject'
      Resource: 'arn:aws:s3:::${env:LESGO_AWS_S3_BUCKET}/*'
```

Learn more about IAM role [here](../../security/iam-role).

## Fetch object

The below example will fetch the object from the default bucket set in the environment variable.

```ts
import { getObject } from 'lesgo/utils/s3';

const objectFile = await getObject('objectKey');
```

The below example will fetch the object from the bucket defined in the second parameter.

```ts
import { getObject } from 'lesgo/utils/s3';

const objectFile = await getObject('objectKey', {
  Bucket: 'myOtherS3Bucket'
});
```
