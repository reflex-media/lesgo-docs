# Deployment

Serverless Framework handles most of the deployment tasks. Lesgo! uses its own deployment script to allow for a more custom deployment process.

## Deploy Entire Application

This command will deploy the entire application to a specific environment.

```bash
npm run deploy -- -s {environment}
```

### Example deploy

```bash
npm run deploy -- -s dev
```

## Deploy Single Function

This command will deploy only a single function to a specific environment.

```bash
npm run deploy -- -s {environment} -f {function_name}
```

### Example deploy

```bash
npm run deploy -- -s dev -f ping
```

## Other Available Commands

These commands are also available.

### Invoke a function

This command will invoke/trigger a single function.

```bash
npm run invoke -- -s {environment} -f {function_name}

# Example
npm run invoke -- -s dev -f ping
```

### Tail log of a function

This command allows you to tail the log of a single function.

```bash
npm run logs -- -s {environment} -f {function_name}

# Example
npm run logs -- -s dev -f ping
```

### Build bundle without deployment

This command allows you to build the bundle without doing actual deployment. This might be useful to note the created bundle files and sizes.

```bash
npm run build -- -s {environment}

# Example
npm run build -- -s dev
```

### Destroy

Destroy everything, leave nothing behind.

!!! danger "DANGER!"
    Note that this action is non-reversible! Everything will disappear from AWS.

```bash
npm run destroy -- -s {environment}

# Example
npm run destroy -- -s dev
```
