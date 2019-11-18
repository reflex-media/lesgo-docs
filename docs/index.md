# Lesgo!

## A lightweight node.js serverless framework

Bootstrap your next microservice with a light-weight node.js serverless framework on AWS.

## Quick Start

!!! info "Prerequisites"
    Install Serverless Framework globally with: `npm install -g serverless`.  
    Refer to https://serverless.com/framework/docs/getting-started/ for additional info.

Create Serverless project:

```bash
sls create --template-url https://github.com/reflex-media/lesgo/tree/master --path my-service
cd my-service
```

Install dependencies:

```bash
yarn install
```

Start local:

```bash
yarn start
```

Access local url via browser or Postman: [http://localhost:8181/ping](http://localhost:8181/ping).
