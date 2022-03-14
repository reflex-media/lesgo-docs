# Directory Structure

```
├── config
|   ├── environments
|   |   ├── .env
|   |   ├── .env.local
|   |   ├── .env.dev
|   |   ├── .env.sandbox
|   |   └── .env.prod
|   ├── functions
|   ├── resources
|   └── utils
├── documents
|   └── Postman
|       ├── Collections
|       └── Environments
├── src
|   ├── config
|   ├── data
|   ├── core
|   ├── exceptions
|   ├── handlers
|   ├── middlewares
|   ├── models
|   ├── services
|   └── utils
└── tests
```

## The Config Directory

The `config/` directory contains the serverless configurations. The application-specific configs can be found in `src/config/` directory instead.

### Environment Config

The `config/environments` directory contains environment-specific configurations. The environment files are used for both deployment and within application code.

You may overwrite env files during a deployment by adding a `.local` suffix e.g; `.env.dev.local`. This is useful for when you want to deploy to a specific environment but not wanting to overwrite committed values.

> `.env`: default environment, served as a local example.  
> `.env.local`: local environment configuration. This should not be committed.  
> `.env.dev`: development environment configuration.  
> `.env.sandbox`: sandbox environment configuration.  
> `.env.prod`: production environment configuration.

### Function Config

The `config/functions/` directory contains the available and declared Serverless functions.

### Resource Config
The `config/resources/` directory contains the available and declared Serverless resources.

### Util Config
The `config/utils/` directory contains additional Serverless configs where required.

## The Documents Directory
The `documents/` directory contains any documents outside of the application. One use case is to store the exported Postman Collection and Environment files here.

Included are the Postman Contract Tests and Endpoints Collection. Import them to your local Postman app and try them out!

## The Source Directory
The `src/` directory contains the main source code for your application.

### Config Directory
The `src/config/` directory contains the application configurations.

### Data Directory
The `src/data/` directory to store mocked data or application schema.

### Core Directory
The `src/core/` directory contains your application's business / functional logic.

### Exception Directory 
The `src/exceptions/` directory contains error classes.

### Handler Directory
The `src/handlers/` directory contains the entry point for all events.

### Middleware Directory
The `src/middlewares/` directory contains the request middlewares.

### Model Directory
The `src/models/` directory contains the Model for the application. Models is the gateway to the database / data store.

### Service Directory
The `src/services/` directory contains class-based services or modules, usually instantiated. These classes are usually made available in the `src/utils/` as helper functions.

### Util Directory
The `src/utils/` directory contains helper functions.

## Test Directory
The `tests/` directory contains test `.spec.js` files for unit testing.
