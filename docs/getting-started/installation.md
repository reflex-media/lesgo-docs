# Installation

Set up your local environment.

## Requirements

Lesgo! is designed to run on AWS, specifically using the Gateway API for all API endpoints, and Lambda for all functions.

Lesgo! also integrates with major AWS services to get a fully functioning and lightweight NodeJS REST application.

## Quick Start

> **Prerequisites**: Install Serverless Framework globally with: `npm install -g serverless`.

Create Serverless project

```bash
$ sls create --template-url https://github.com/reflex-media/lesgo/tree/master --path my-service

$ cd my-service
```

Install dependencies

```bash
$ yarn install
```

Start local

```bash
$ yarn start
```

Access local url via browser or Postman (recommended): http://localhost:8181/ping.

## Available Endpoints

**`/ping`**  
Send a "liveness-check" request to your endpoint.

**`/ping/auth`**  
Send a "liveness-check" request with a required authorization.  
**Note**: Set `x-api-key` in your request header for a valid request.

**`/ping?sample-error=message`**  
Returns the request with an "error message" response type.

**`/ping?sample-error=exception`**  
Returns the request with an "error class" response type.
