# Cache

Lesgo! uses AWS ElastiCache for caching.

## Configuration

This is configurable in the `src/config/cache.js`.

```js
export default {
  default: "memcached",
  connections: {
    memcached: {
      url: process.env.ELASTICACHE_MEMCACHED_URL || null,
      options: {
        autoDiscover: true,
        autoDiscoverInterval: 60000,
        autoDiscoverOverridesRemove: false
      }
    }
  }
};
```

ElastiCache is disabled by default. To enable ElastiCache, follow the steps below.

1. Uncomment `elastiCache.yml` resource in `serverless.yml`

```yml
resources:
  - ${file(${self:custom.path.resources}/elastiCache.yml)}
```

2. Deploy application and retrieve ElastiCache node Endpoint on AWS Console.

```apache
yarn deploy -s development
```

3. Update relevant environment file in `config/environments/` directory.

```apache
ELASTICACHE_MEMCACHED_URL="INSERT_ELASTICACHE_NODE_ENDPOINT_HERE"
```

Deploy your application again and you may now use ElastiCache.

## Cache Usage

Import the `cache` module from `Utils/cache`.

To test the sample usage, uncomment `samples.yml` function in `serverless.yml` and deploy the application to a development environment.

### Retrieving items from the cache

You may use the `get` method on the `cache` util to fetch items from the cache.

```js
import cache from "Utils/cache";

const cacheKey = "foo";
const data = await cache.get(cacheKey);
```

Test sample usage with this url endpoint `/samples/cache?method=get&key=sampleCacheKey`.

### Storing items to the cache

You may use the `set` method on the `cache` util to store items to the cache.

```js
import cache from "Utils/cache";

const cacheKey = "foo";
const cacheValue = "bar";
const cacheLifetimeInSeconds = 10;

const data = await cache.set(cacheKey, cacheValue, cacheLifetimeInSeconds);
```

Test sample usage with this url endpoint `/samples/cache?method=set&key=sampleCacheKey&value=sampleCacheValue`.

### Deleting items from the cache

You may use the `del` method on the `cache` util to delete items from the cache.

```js
import cache from "Utils/cache";

const cacheKey = "foo";
const data = await cache.del(cacheKey);
```

Test sample usage with this url endpoint `/samples/cache?method=del&key=sampleCacheKey`.
