# Lesgo!

## A lightweight node.js boilerplate framework for serverless architecture

Bootstrap your next serverless microservice with a light-weight node.js app built on top of the [Serverless Framework](https://www.serverless.com/) on AWS.

## Quick Start

!!! info "Prerequisites"

    Install Serverless Framework globally with: `npm install -g serverless`.
    Refer to https://serverless.com/framework/docs/getting-started/ for additional info.

Create Serverless project:

```bash
sls create --template-url https://github.com/reflex-media/lesgo-lite/tree/master --path my-service
cd my-service
```

Install dependencies:

```bash
npm install
```

Start local:

```bash
npm start
```

Access local url via browser or Postman: [http://localhost:8181/ping](http://localhost:8181/ping).

## Why Lesgo! Framework

Like any other frameworks out there, we built this Framework because we couldn't find one that fits our needs. We believe that a framework for the Serverless Architecture should fit the following criteria:

- **Super lightweight**. The bundled lambda function should only bundle what is necessarily required by the function, and nothing else. This is why we chose the Serverless Framework as the basic building block for this framework. It provides the necessary tools to build, bundle, and deploy direct to AWS via CloudFormation, while still allowing the option to bundle each function individually.
- **Easy to understand file structure**. Most of our file structure, scripts, and helper functions is inspired from the Laravel Framework. The primary reason being that all the original developers and current maintainers have years of experience building applications on Taylor Otwell's Laravel Framework. If you know Laravel, you will know Lesgo!
- **Highly adaptable for any use case**. We have built multiple microservices for the last 3 years from a simple "Unique Name Generator", to a process-intensive "Photo Processor" and "Image Moderation", to complex projects building a Social Network App using this very same framework!
- **Continous upgrades**. As long as we continue to use this framework, we will continue to maintain and expand its functionalities. This framework will continue to grow with us. We take the learnings from our other microservices and upgrade this framework accordingly.
