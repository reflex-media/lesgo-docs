# Application Configuration

The app's configuration can be managed on the `src/config` directory.

## App Config vs Env Variables

The App Config will be used across the entire application source code while the Env Variables can be used to set the App Config.

For example, you may want to set debug mode only on non-production environments while ensuring debug mode is disabled on production environment.

```bash
export default {
  debug: process.env.APP_DEBUG === 'true'
}
```

!!! info "Environment variables type"
    All values coming from the `process.env` are presented as a string type. As such, it is also important to take note of type conversion when being used on the app config.

## Available App Config

```typescript
// src/config/app.ts

export default {
  // Define the name of the service / app
  name: process.env.APP_NAME,

  // Define the environment / stage
  env: process.env.APP_ENV || process.env.NODE_ENV,

  // Set to true to enable debug logs and return metadata in the response;
  debug: process.env.APP_DEBUG === 'true',
};
```

There are other App Configuration that may be required. However, this is dependent on the modules being used.
