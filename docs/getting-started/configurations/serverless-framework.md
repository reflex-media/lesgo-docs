# Serverless Framework Configuration

The Serverless Framework configurations consists of `.yml` files and are located within the `config/functions` and `config/resources` directory. The main Serverless Framework configurations can also be found in the root as `serverless.yml`.

## Serverless Framework Main Configuration

This is the main config file for the Serverless Framework. Learn more from the official [Serverless Framework docs](https://www.serverless.com/framework/docs).

Additionally, here are some useful pointers to highlight.

### Use of variables

Use variables where values are repeatedly used. Variables are also useful when it comes to dynamic fields like from DOTenv or dynamically generated data like from a CloudFormation stack.

#### Cheatsheet

```apache
# Use ${env:<key>} to reference keys from DOTenv
# ${env:APP_NAME} will reference the APP_NAME from DOTenv
${env:APP_NAME}

# Use ${self:<key>} to reference its own keys
# ${self:service} will reference the service: key within its own file
# ${self:provider.stage} will reference the provider.stage: key within its own file
${self:service}-${self:provider.stage}

# Use ${opt:<key>} to reference parameters declared from command line
# ${opt:stage} will reference the --stage parameter declared from command line
${opt:stage}

# Declare the 2nd parameter as a fallback should the first parameter does not exist or is empty.
# Value of 'local' will be used when the --stage parameter was not declared from command line
${opt:stage, 'local'}
```

### Functions Declaration

The functions are declared within the `config/functions` directory. Separate each module within its own `config/functions/<moduleName>.yml` file for better organization.

Be sure to import the functions within the main `serverless.yml` file. Refer to the samples provided.

### Resources Declaration

For any custom resources that you'd want to create, add them to the `config/resources` directory. Separate each resource type within its own `config/resources/resource.yml` file for better orgnization.

Be sure to import the resources within the main `serverless.yml` file. Refer to the samples provided.