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

1. Uncomment `iamRoleStatement` for `elasticache` in `serverless.yml`

   ```yml
   iamRoleStatements:
     - Effect: "Allow"
       Action:
         - "elasticache:*"
       Resource: "*"
   ```

2. Uncomment `elastiCache.yml` resource in `serverless.yml`
   ```yml
   resources:
     - ${file(${self:custom.path.resources}/elastiCache.yml)}
   ```

3. Deploy application and retrieve ElastiCache node Endpoint on AWS Console.

4. Update relevant environment file in `config/environments/` directory.
   ```bash
   ELASTICACHE_MEMCACHED_URL="INSERT_ELASTICACHE_NODE_ENDPOINT_HERE"
   ```

Deploy your application again and you may now use ElastiCache.

## Cache Usage

Import the `cache` module from `Utils/cache`.

### Retrieving items from the cache

You may use the `get` method on the `cache` util to fetch items from the cache.

```js
import cache from "Utils/cache";

const cacheKey = "foo";
const data = await cache.get(cacheKey);
```

See usage sample with this url endpoint `/ping?sample-cache=get&key=sampleCacheKey`.

### Storing items to the cache

You may use the `set` method on the `cache` util to store items to the cache.

```js
import cache from "Utils/cache";

const cacheKey = "foo";
const cacheValue = "bar";
const cacheLifetimeInSeconds = 10;

const data = await cache.set(cacheKey, cacheValue, cacheLifetimeInSeconds);
```

See usage sample with this url endpoint `/ping?sample-cache=set&key=sampleCacheKey&value=sampleCacheValue`.

### Deleting items from the cache

You may use the `del` method on the `cache` util to delete items from the cache.

```js
import cache from "Utils/cache";

const cacheKey = "foo";
const data = await cache.del(cacheKey);
```

See usage sample with this url endpoint `/ping?sample-cache=del&key=sampleCacheKey`.
