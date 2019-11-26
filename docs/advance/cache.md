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

## Cache Usage

### Retrieving items from the cache

You may use the `get` method on the `cache` util to fetch items from the cache.

```js
import cache from "Utils/cache";

const cacheKey = "foo";
const data = await cache.get(cacheKey);
```

### Storing items to the cache

You may use the `set` method on the `cache` util to store items to the cache.

```js
import cache from "Utils/cache";

const cacheKey = "foo";
const cacheValue = "bar";
const cacheLifetimeInSeconds = 10;

const data = await cache.set(cacheKey, cacheValue, cacheLifetimeInSeconds);
```

### Deleting items from the cache

You may use the `del` method on the `cache` util to delete items from the cache.

```js
import cache from "Utils/cache";

const cacheKey = "foo";
const data = await cache.del(cacheKey);
```
