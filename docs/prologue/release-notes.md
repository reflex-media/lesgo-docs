# Release Notes

## Multi SLS configuration support

Lesgo! now supports multiple `serverless.yml` configuration to exist within the same codebase. This allows you to deploy separate applications. Useful for cases where you might have exceeded the resource allocation.

Simply create a new `serverless-x.yml` file, update the configurations accordingly, add a new `deploy-x` script in `package.json`, specifying the config file to deploy with `-c "severless-x.yml"`

**Sample package.json**

```json
{
    ...,
    "scripts": {
        ...,
        "deploy": "lesgo-scripts -t deploy",
        "deploy-backend": "lesgo-scripts -t deploy -c \"./serverless-x.yml\"",
        ...,
    },
    ...
}
```

**Sample Deployment**

```apache
npm run deploy-x -- -s dev
```

## Disconnecting of Persistent Connections

Previously, persistent connections have to be manually disconnected for both db and cache connections. As long as you want to have persistent connection, add that to the Handler and pass it along to the middlewares.

**Sample usage with middleware**

```js
// src/handlers/utils/ping.js

import middy from "@middy/core";
import httpMiddleware from "lesgo/middlewares/httpMiddleware";
import db from "lesgo/utils/db";
import ping from "core/utils/ping";

const originalHandler = async (event) => {
  await Promise.all([dbRead.pConnect(), cache.pConnect()]); // connect to the db and cache before anything else

  return ping(event.input);
};

export const handler = middy(originalHandler);

handler.use(httpMiddleware({ db, cache })); // pass along the instance to the middleware
```

**Sample usage without middleware**

```js
// src/handlers/utils/ping.js

import middy from "@middy/core";
import db from "lesgo/utils/db";
import ping from "core/utils/ping";

const originalHandler = async (event) => {
  await db.pConnect(); // connect to the db before anything else

  try {
    const resp = await ping(event.input); // important to await here to prevent premature disconnection
    return resp;
  } catch (err) {
    throw err;
  } finally {
    db.end(); // disconnect after everything else is done
  }
};

export const handler = middy(originalHandler);

handler.use();
```

## memcache-plus over memcached-elasticache

memcached-elasticache is no longer being actively maintained and has been unstable especially with timeout disconnections. As such, [memcache-plus](https://www.npmjs.com/package/memcache-plus) is the new SDK for AWS Elasticache Memcached service.

The `src/config/cache.js` should be updated accordingly.

```js
export default {
  default: "memcached",
  connections: {
    memcached: {
      options: {
        hosts: [
          process.env.ELASTICACHE_MEMCACHED_URL ||
            "api-dev-cache.xdw723.cfg.uswest2.cache.amazonaws.com:11211",
        ],
        autoDiscover: true,
      },
    },
  },
};
```

## Additional Methods

### Utils/cache

- Retrieve multiple cache data with `cache.getMulti()`. [Learn more](../advance/cache.md#retrieving-multiple-data-from-the-cache)
- Delete multiple cache data with `cache.delMulti()`. [Learn more](../advance/cache.md#deleting-multiple-cache-data)

### Utils/elasticsearch

Added a new method to execute multiple different queries using a single API request with `es.msearch()`. [Learn more](../packages/elasticsearch.md#esmsearch).

## Additional Notes

### Elasticsearch 7.13.0

It has been discovered that `@elastic/elasticsearch` greater than `@7.13.0` is not compatible with the SDK. Thus is is important to ensure the package version is set to 7.13.0.

### Removal of db config persists

As part of ensuring file size optimization (and thus improving function initialization performance), usage of `persists` key has now been deprecated and is strongly advised to be removed in the `config/db.connections.rdsProxy` and `config/db.connections.rdsProxy`.

Persistent Connections should be added on a as needed basis in the Handlers and passed along to the middlewares. See [Persistent Connection for RDS](../database/rds-aurora.md#persistent-connection-for-rds-proxy) Proxt for more info.
