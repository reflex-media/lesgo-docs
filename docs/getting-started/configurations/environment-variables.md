# Environment Variables

It is often helpful to have different configuration values based on the environment where the application is running. For example, you may wish to use a different SQS queue on a testing server than you do on your production server.

To make this happen, Lesgo! uses the Serverless DOTenv plugin. DOTenv files are stored in `config/environments/` directory. The supported environments are currently `local`, `dev`, `sandbox`, `prod`. However, you may declare as many `DOTenv` environments as needed.

These environment files can be committed to the source control. To overwrite for your own local build, you may create a local DOTenv as such example: `.env.dev.local`. This will allow you to overwrite the existing `.env.dev` without having to commit it.

You may overwrite env variables during a deployment by adding a `.local` suffix e.g; `.env.dev.local`. This is useful for when you want to deploy to a specific environment but not wanting to overwrite committed values.

| File Name         | Description                                       | Commit Status           |
|-------------------|---------------------------------------------------|-------------------------|
| `.env`            | Default environment, served as a local example.   | Can be committed        |
| `.env.local`      | Local environment variables.                      | Should not be committed |
| `.env.dev`        | Development environment variables.                | Can be committed        |
| `.env.dev.local`  | Development environment based on local variables. | Should not be committed |
| `.env.sandbox`    | Sandbox environment variables.                    | Can be committed        |
| `.env.prod`       | Production environment variables.                 | Can be committed        |

!!! danger "Secret keys"
    Secret or sensitive keys should not be committed to the DOTenv files. Store them in services like the AWS Parameter Store or AWS Secrets Manager instead.

## Available Environment Variables

The following environment variables are required to run the basic app.

```bash
# src/config/environments/.env

# Declare the name of the application
APP_ENV=lesgo-app

# Declare the stage to deploy to
APP_ENV=dev

# Enable/disable debug mode. Recommended to set to false on prod env
APP_DEBUG=true

# Determine the region to deploy to
AWS_ACCOUNT_REGION=us-west-1

# This name needs to match the aws credentials profile on your local machine.
AWS_ACCOUNT_PROFILE=slsDevProfile

# The AWS account id being deployed to
AWS_ACCOUNT_ID=
```

There are other environment variables that may be required. This will be mentioned on the modules being used.
