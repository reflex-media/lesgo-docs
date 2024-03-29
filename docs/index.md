# Installation

## Lesgo! Framework

A lightweight node.js boilerplate framework for serverless architecture.

Bootstrap your next serverless microservice with a light-weight node.js app built on top of the [Serverless Framework](https://www.serverless.com/) on AWS.

### Why Lesgo! Framework?

Like any other frameworks out there, we built this Framework because we couldn't find one that fits our needs. We believe that a framework for the Serverless Architecture should fit the following criteria:

- **Super lightweight**. The bundled lambda function should only bundle what is necessarily required by the function, and nothing else. This is why we chose the Serverless Framework as the basic building block for this framework. It provides the necessary tools to build, bundle, and deploy direct to AWS via CloudFormation, while still allowing the option to bundle each function individually.
- **Easy to understand file structure**. Most of our file structure, scripts, and helper functions is inspired from the Laravel Framework. The primary reason being that all the original developers and current maintainers have years of experience building applications on Taylor Otwell's Laravel Framework. If you know Laravel, you will know Lesgo!
- **Highly adaptable for any use case**. We have built multiple microservices for the last 3 years from a simple "Unique Name Generator", to a process-intensive "Photo Processor" and "Image Moderation", to complex projects building a Social Network App using this very same framework!
- **Continous upgrades**. As long as we continue to use this framework, we will continue to maintain and expand its functionalities. This framework will continue to grow with us. We take the learnings from our other microservices and upgrade this framework accordingly.

## Quick Start

!!! info "Prerequisites"

    Install Serverless Framework globally with: `npm install -g serverless`.
    Refer to https://serverless.com/framework/docs/getting-started/ for additional info.

Create Serverless project:

```bash
sls create --template-url https://github.com/reflex-media/lesgo-lite/tree/master --path my-service
cd my-service
```

Install dependencies:

```bash
npm install
```

Start local:

```bash
npm start
```

Access local url via browser or Postman: [http://localhost:8181/ping](http://localhost:8181/ping).

## Configuration

There are 2 levels of configurations for the Lesgo! framework.

The project (serverless) configurations are stored in `config/` directory as `.yml` files. These configuration files affect your project set up and build.

The application configurations are stored in `src/config/` directory as `.js` files (We'll move to TypeScript soon, we promise!). These are application/business specific configurations.

Each configuration is documented below, so feel free to look through the files and get familiar with the options relevant to you.

### Environment Configuration

It is often helpful to have different configuration values based on the environment where the application is running. For example, you may wish to use a different SQS queue locally than you do on your production server.

To make this happen, Lesgo! uses the Serverless DOTenv plugin. DOTenv files are stored in `config/environments/` directory. The supported environments are currently `local`, `dev`, `sandbox`, `prod`.

These environment files can be committed to the source control. To overwrite for your local build, you may create a local DOTenv as such example: `.env.dev.local`. This will allow you to overwrite the existing `.env.dev` without having to commit it.

### Available Environment Configurations

```apache
# Declare the environment to deploy to
APP_ENV="dev"

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

# Maximum size before gzip compression for response
AWS_APIGATEWAY_COMPRESSION_MAX_BYTES=
```
