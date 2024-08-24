# Cache

Lesgo! uses AWS ElastiCache for caching. Currently only Redis cache is supported.

## Pre-requisites

In order to start using cache (via AWS ElastiCache - Redis), set it up directly from the [AWS Console](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/LambdaRedis.step1.html).

!!! info "ElastiCache requires VPC"

    AWS ElastiCache must exists within a VPC. [Learn more](../../security/vpc) on setting up VPC for your microservice.

## Configuration

Update the following environment variables

```bash
# Set the AWS ElastiCache Redis endpoint uri
LESGO_AWS_ELASTICACHE_REDIS_ENDPOINT=

# Set the AWS ElastiCache Redis port number
LESGO_AWS_ELASTICACHE_REDIS_PORT=
```

## Retrieving data from the cache

```ts
import { getCache } from 'lesgo/utils/cache/redis';

const data = await getCache('foo');
```

## Storing data to the cache

```ts
import { setCache } from 'lesgo/utils/cache/redis';

const cacheKey = 'foo';
const cacheValue = 'bar';

await setCache(cacheKey, cacheValue);
```

To set expiry, enter the expiry time as the 3rd parameter.


```ts
import { setCache } from 'lesgo/utils/cache/redis';

const cacheKey = 'foo';
const cacheValue = 'bar';
const cacheExpire = 15; // in seconds. Defaults to 5 min expiry if not defined

await setCache(cacheKey, cacheValue, {
  EX: cacheExpire
});
```


## Deleting cache data

```ts
import { deleteCache } from 'lesgo/utils/cache/redis';

await deleteCache('foo');
```
