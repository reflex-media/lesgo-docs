# IAM Role

Serverless Framework will create a new IAM Role for every new service. This role will be used to access resources. Permissions can be added to this role when required.

!!! danger "Restrict permissions"

    While it is tempting to simply add all permissions for this role, it is recommended to only add them as needed.

## Permissions

Permissions can be added directly on your `serverless.yml` file.

### Adding permission

The below permission allows the lambda to get and put objects from and to the S3 bucket.

```yaml
provider:
  ...
  iamRoleStatements:
    - Effect: 'Allow'
      Action:
        - 's3:GetObject'
        - 's3:PutObject'
      Resource: 'arn:aws:s3:::${env:LESGO_AWS_S3_BUCKET}/*'
```
