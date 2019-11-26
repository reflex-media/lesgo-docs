# Deployment

Serverless Framework handles most of the deployment tasks. Lesgo! uses its own deployment script to allow for a more custom deployment process.

## Deploy Entire Application

This command will deploy the entire application to a specific environment.

```bash
yarn deploy -s {environment}
```

### Example deploy

```bash
yarn deploy -s development
```

## Deploy Single Function

This command will deploy only a single function to a specific environment.

```bash
yarn deploy -s {environment} -f {function_name}
```

### Example deploy

```bash
yarn deploy -s development -f Ping
```

## Other Available Commands

These commands are also available.

### Invoke a function

This command will invoke/trigger a single function.

```bash
yarn invoke -s {environment} -f {function_name}

# Example
yarn invoke -s development -f Ping
```

### Tail log of a function

This command allows you to tail the log of a single function.

```bash
yarn logs -s {environment} -f {function_name}

# Example
yarn logs -s development -f Ping
```

### Build bundle without deployment

This command allows you to build the bundle without doing actual deployment. This might be useful to note the created bundle files and sizes.

```bash
yarn build -s {environment}

# Example
yarn build -s development
```

### Destroy

Destroy everything, leave nothing behind.

!!! danger "DANGER!"
    Note that this action is non-reversible! Everything will disappear from AWS.

```bash
yarn destroy -s {environment}

# Example
yarn destroy -s development
```
