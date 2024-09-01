# Directory Structure

The directory structure of Lesgo! Framework is inspired by Laravel Framework.

```
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

## Directory Structure

| Directory                  | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| `utils`                    | Contains the serverless configurations.                                              |
| `documents`                | Contains any documents outside of the application.                          |
| `src`                      | Source code directory.                                                      |
| `src/config`               | Application-specific configurations.                                        |
| `src/core`                 | Core application logic.                                                     |
| `src/exceptions`           | Custom exceptions and error handling.                                       |
| `src/handlers`             | Lambda function handlers.                                                   |
| `src/middlewares`          | Middleware functions for request processing.                                |
| `src/models`               | Data models and schemas.                                                    |
| `src/services`             | Business logic and service classes.                                         |
| `src/utils`                | Additional utility functions and helpers.                                   |

## Configuration Directories

| Directory                  | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| `config/`                  | Contains the serverless configurations.                                     |
| `config/environments`      | Environment-specific configurations.                                        |
| `config/functions/`        | Available and declared Serverless functions.                                |
| `config/resources/`        | Available and declared Serverless resources.                                |
| `config/utils/`            | Additional Serverless configurations where required.                        |

Refer to [Environment Variables](../configurations/environment-variables/) for more info.
