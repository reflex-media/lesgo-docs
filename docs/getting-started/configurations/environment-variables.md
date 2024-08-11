# Environment Variables

It is often helpful to have different configuration values based on the environment where the application is running. For example, you may wish to use a different SQS queue on a testing server than you do on your production server.

To make this happen, Lesgo! uses the Serverless DOTenv plugin. DOTenv files are stored in `config/environments/` directory. The supported environments are currently `local`, `dev`, `sandbox`, `prod`. However, you may declare as many `DOTenv` environments as needed.

These environment files can be committed to the source control. To overwrite for your own local build, you may create a local DOTenv as such example: `.env.dev.local`. This will allow you to overwrite the existing `.env.dev` without having to commit it.

You may overwrite env variables during a deployment by adding a `.local` suffix e.g; `.env.dev.local`. This is useful for when you want to deploy to a specific environment but not wanting to overwrite committed values.

> `.env`: default environment, served as a local example.  
> `.env.local`: local environment variables. This should not be committed.  
> `.env.dev`: development environment variables.
> `.env.dev.local`: development environment based on local variables. This should not be committed.
> `.env.sandbox`: sandbox environment variables.  
> `.env.prod`: production environment variables.

!!! danger Sensitive keys
    Secret or sensitive keys should not be committed to the DOTenv files. Store them in services like the AWS Parameter Store or AWS Secrets Manager instead.

## Available Environment Variables

The following environment variables are required to run the basic app.

```bash
# config/environments/.env.dev.local

# Declare the name of the application
APP_ENV=lesgo-app

# Declare the environment to deploy to
APP_ENV=dev

# Enable/disable debug mode. Recommended set to false on prod env.
APP_DEBUG=true

# Determine the region to deploy to
AWS_ACCOUNT_REGION=us-west-1

# This name needs to match the aws credentials profile on your local machine.
AWS_ACCOUNT_PROFILE=slsDevProfile

# The AWS account id being deployed to
AWS_ACCOUNT_ID=
```

There are other environment variables that may be required. However, this is dependent on the modules being used.