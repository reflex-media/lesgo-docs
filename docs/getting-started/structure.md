# Directory Structure

```
├── config
|   ├── environments
|   |   ├── .env
|   |   ├── .env.local
|   |   ├── .env.development
|   |   ├── .env.staging
|   |   └── .env.production
|   ├── functions
|   ├── resources
|   └── utils
├── src
|   ├── config
|   |   ├── app.js
|   |   ├── aws.js
|   |   ├── index.js
|   |   └── sentry.js
|   ├── constants
|   ├── core
|   ├── exceptions
|   ├── handlers
|   ├── middlewares
|   ├── services
|   └── utils
└── tests
```

## The Config Directory

The `config/` directory contains the serverless configurations. The application-specific configs can be found in `src/config/` directory instead.

### Environment Config

The `config/environments` directory contains environment-specific configurations. The environment files are used for both deployment and within application code.

You may overwrite env files during a deployment by adding a `.local` suffix e.g; `.env.development.local`. This is useful for when you want to deploy to a specific environment but not wanting to overwrite committed values.

> `.env`: default environment, served as a local example.  
> `.env.local`: local environment configuration. This should not be committed.  
> `.env.development`: development environment configuration.  
> `.env.staging`: staging environment configuration.  
> `.env.production`: production environment configuration.

### Function Config

The `config/functions/` directory contains the available and declared Serverless functions.

### Resource Config
The `config/resources/` directory contains the available and declared Serverless resources.

### Util Config
The `config/utils/` directory contains additional Serverless configs where required.

## The Source Directory

The `src/` directory contains the main source code for your application.

### Config Directory

The `src/config/` directory contains the application configurations.

### Constant Directory
The `src/constants/` directory contains any application-specific constants made available throughout your source code.

### Core Directory
The `src/core/` directory contains your application's core business logic.

### Exception Directory 
The `src/exceptions/` directory contains error classes.

### Handler Directory
The `/src/handlers/` directory contains the entry point for all events.

### Middleware Directory
The `/src/middlewares/` directory contains the request middlewares.

### Service Directory
The `/src/services/` directory contains class-based services or modules, usually instantiated. These classes are usually made available in the `src/utils/` as helper functions.

### Util Directory
The `/src/utils/` direcotry contains helper functions.

## Test Directory
The `/tests/` directory contains test `.spec.js` files for unit testing.
