# Directory Structure

The directory structure of Lesgo! Framework is inspired by Laravel Framework.

```bash
├── config
|   ├── environments
|   |   ├── .env
|   |   ├── .env.local
|   |   ├── .env.dev
|   |   ├── .env.dev.local
|   |   ├── .env.sandbox
|   |   └── .env.prod
|   ├── functions
|   |   └── utils.yml
|   ├── resources
|   └── utils
├── documents
└── src
    ├── config
    ├── core
    ├── exceptions
    ├── handlers
    ├── middlewares
    ├── models
    ├── services
    └── utils
```

## The Config Directory

The `config/` directory contains the serverless configurations. The application-specific configs can be found in `src/config/` directory instead.

### Environment Config

The `config/environments` directory contains environment-specific configurations. The environment files are used for both deployment and within application code via the App Config.

Refer to [Environment Variables](../configurations/environment-variables/) for more info.

### Function Config

The `config/functions/` directory contains the available and declared Serverless functions.

### Resource Config
The `config/resources/` directory contains the available and declared Serverless resources.

### Util Config
The `config/utils/` directory contains additional Serverless configs where required.

## The Documents Directory
The `documents/` directory contains any documents outside of the application. One use case is to store the exported Postman Collection and Environment files here.

## The Source Directory
The `src/` directory contains the main source code for your application.

### Config Directory
The `src/config/` directory contains the application configurations.

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

### Utils Directory
The `src/utils/` directory contains helper functions.
