# Deployment

Serverless Framework handles most of the deployment tasks. Lesgo! uses its own deployment script to allow for a more custom deployment process.

## Pre-requisites

Before you are able to deploy to the AWS environment, you need to ensure you have the AWS credentials configured within your local machine.

### Install AWS CLI

```bash
brew install awscli
```

### Verify installation

After installation, verify that the AWS CLI is installed and accessible globally

```bash
aws --version
```

### Configure AWS CLI

After installation, you might want to configure the AWS CLI with your credentials:

```bash
aws configure
```

This will prompt you to enter your AWS Access Key ID, Secret Access Key, region, and output format.

!!! info AWS Credentials
    You may obtain the AWS Credentials from AWS IAM for programmatic access.

After completing these steps, the AWS CLI will be installed globally on your system.

## Deploy Entire Application

This command will deploy the entire application to a specific environment.

```bash
npm run deploy -- -s dev
```

## Deploy Single Function

This command will deploy only a single function to a specific environment.

```bash
npm run deploy -- -s dev -f ping
```

## Other Available Commands

These commands are also available.

### Invoke a function

This command will invoke/trigger a single function.

```bash
npm run invoke -- -s dev -f ping
```

### Tail log of a function

This command allows you to tail the log of a single function.

```bash
npm run logs -- -s dev -f ping
```

### Build bundle without deployment

This command allows you to build the bundle without doing actual deployment. This might be useful to note the created bundle files and sizes.

```bash
npm run build -- -s dev
```

### Destroy

Destroy everything, leave nothing behind.

!!! danger "DANGER!"

    Note that this action is non-reversible! Everything will disappear from AWS including DynamoDb, RDS, etc.

```bash
npm run destroy -- -s dev
```
