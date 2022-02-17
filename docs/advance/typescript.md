# Typescript

Lesgo! can be integrated with a TS code-base.

## Installation

To enable typescript for Lesgo!, follow the steps below.

1. Install @types package
```bash
npm install -D @types/lesgo
```

2. Rename `jsconfig.json` to `tsconfig.json` and verify contents to have these:
```json
{
  "compilerOptions": {
    ...
    "paths": {
      ...
      "Exceptions/*": [
        ...
        "node_modules/@types/lesgo/exceptions/*"
      ],
      "Middlewares/*": [
        ...
        "node_modules/@types/lesgo/middlewares/*"
      ],
      "Services/*": [
        ...
        "node_modules/@types/lesgo/services/*"
      ],
      "Utils/*": [
        ...
        "node_modules/@types/lesgo/utils/*"
      ]
    }
  }
}
```