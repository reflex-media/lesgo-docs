# Cache

Lesgo! uses AWS ElastiCache for caching.

## Configuration

This is configurable in the `src/config/cache.js`. Or copy [this file](https://raw.githubusercontent.com/reflex-media/lesgo/master/src/config/cache.js) to that path.

```js
export default {
  default: "memcached",
  connections: {
    memcached: {
      url: process.env.ELASTICACHE_MEMCACHED_URL || null,
      options: {
        autoDiscover: true,
      },
    },
  },
};
```

ElastiCache is disabled by default. To enable ElastiCache, follow the steps below.

1. Uncomment `elastiCache.yml` resource in `serverless.yml`

```yml
resources:
  - ${file(${self:custom.path.resources}/elastiCache.yml)}
```

2. Deploy application and retrieve ElastiCache node Endpoint on AWS Console.

```bash
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

### Retrieving data from the cache

`Utils/cache.get()` will return data from given cache key.

```js
cache.get(
  key: String, // cache key to fetch data from
): Promise;
```

```js
import cache from "Utils/cache";

const data = await cache.get("foo");
```

### Retrieving multiple data from the cache

`Utils/cache.getMulti()` will return multiple data from multiple cache keys.

```js
cache.getMulti(
  keys: String[], // array of cache keys to fetch data from
): Promise;
```

**Example Usage**

```js
import cache from "Utils/cache";

const cacheKeys = ["foo", "foo2", "foo3"];

const data = await cache.getMulti(["foo", "foo2", "foo3"]);
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

Test sample usage with this url endpoint `/samples/cache?method=set&key=sampleCacheKey&value=sampleCacheValue`.

### Deleting cache data

`Utils/cache.del()` will remove the cache key.

```js
cache.del(
  key: String, // cache key to remove
): Promise;
```

**Example Usage**

```js
import cache from "Utils/cache";

await cache.del("foo");
```

### Deleting multiple cache data

`Utils/cache.del()` will remove the cache key.

```js
cache.delMulti(
  keys: String[], // array of cache keys to remove
): Promise;
```

**Example Usage**

```js
import cache from "Utils/cache";

await cache.delMulti(["foo", "foo2", "foo3"]);
```
