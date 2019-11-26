# Installation

## Quick Start

!!! info "Prerequisites"
    Install Serverless Framework globally with: `npm install -g serverless`.  
    Refer to https://serverless.com/framework/docs/getting-started/ for additional info.

Create Serverless project:

```bash
sls create --template-url https://github.com/reflex-media/lesgo/tree/master --path my-service
cd my-service
```

Install dependencies:

```bash
yarn install
```

Start local:

```bash
yarn start
```

Access local url via browser or Postman: [http://localhost:8181/ping](http://localhost:8181/ping).

## Configuration

There are 2 levels of configurations for the Lesgo! framework. 

The project configurations are stored in `config/` directory. These configuration files affect your project set up and build.

The application configurations are stored in `src/config/` directory. These are application/business specific configurations.

Each option is documented, so feel free to look through the files and get familiar with the options available to you.

### Environment Configuration

It is often helpful to have different configuration values based on the environment where the application is running. For example, you may wish to use a different SQS queue locally than you do on your production server.

To make this happen, Lesgo! uses the Serverless DOTenv plugin. DOTenv files are stored in `config/environments/` directory. The supported environments are currently `local`, `development`, `staging`, `production`.

These environment files can be committed to the source control. To overwrite for your local build, you may create a local DOTenv as such example: `.env.development.local`. This will allow you to overwrite the existing `.env.development` without having to commit it.

### Available Environment Configurations

```bash
# Declare the environment
APP_ENV="development"

# Enable/disable debug mode
APP_DEBUG=true

# Determine the region to deploy to
AWS_ACCOUNT_REGION="us-west-1"

# This name needs to match the aws credentials profile on your local machine
AWS_ACCOUNT_PROFILE="slsDevProfile"

# Set the default timeout for all lambda functions
AWS_LAMBDA_TIMEOUT=3

# Set the default memory size for all lambda functions
AWS_LAMBDA_MEMORY_SIZE=128

# Set the default retention period for all cloudwatch logs
AWS_LOG_RETENTION_DAYS=7

# Define your own custom API Gateway Secret Key
AWS_APIGATEWAY_SECRET_KEY=

# Maximum size before gzip compression for response
AWS_APIGATEWAY_COMPRESSION_MAX_BYTES=
```