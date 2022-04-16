# Elasticsearch

Lesgo! is pre-configured with AWS Elasticsearch 7.10 engine.

## Configuration

The elasticsearch configuration for your application is located at `src/config/elasticsearch.js`. Or copy [this file](https://raw.githubusercontent.com/reflex-media/lesgo/master/src/config/elasticsearch.js) to that path.

You may also simply update the respective environment files in `config/environments/*` as such:

```apache
# Elasticsearch index name to use
ES_INDEX=""

# Elasticsaerch mapping type
# Empty this line if you are using Elasticsearch ≤ 6
ES_TYPE="_doc"

# Elasticsearch node URL
ES_NODE=""

# Elasticsearch region name
ES_REGION=""
```

## Basic Usage

To run a basic query, you may use the search method on the `Utils/es`:

```js
import es from "Lesgo/Utils/elasticsearch";

const response = await es().search({
  query: {
    match: {
      quote: "winter",
    },
  },
});

/**
{
	"body": {
	    "took": 22,
	    "timed_out": false,
	    "_shards": {
	        "total": 6,
	        "successful": 6,
	        "skipped": 0,
	        "failed": 0
	    },
	    "hits": {
	        "total": 10,
	        "max_score": null,
	        "hits": [
	            {
	            	"_index": "test_search_20210628",
	                "_type": "profiles",
	                "_id": "3692613",
	                "_score": null,
	                "_source": {
	                    "username": "test_user",
	                    "language": "en",
	                    "age": 32,
	                    "height": "157"
	                },
	                "sort": [
	                    1637620268000
	                ]
	            }
	          	...
	        ]
	    }
	}
}
*/
```

## Advanced Query

```js
import es from "Lesgo/Utils/elasticsearch";

const response = await es().search({
  from: 0,
  size: 4,
  query: {
    bool: {
      must: [
        {
          match: {
            account_type: 2,
          },
        },
      ],
      must_not: {
        terms: {
          account_id: [3627950],
        },
      },
      filter: [
        {
          geo_distance: {
            distance: "50mi",
            distance_type: "arc",
            location: {
              lat: 37.7749295,
              lon: -122.4194155,
            },
          },
        },
      ],
    },
  },
  sort: [
    {
      last_activity_dt: {
        order: "desc",
      },
    },
  ],
});

/**
{
	"body": {
	    "took": 22,
	    "timed_out": false,
	    "_shards": {
	        "total": 6,
	        "successful": 6,
	        "skipped": 0,
	        "failed": 0
	    },
	    "hits": {
	        "total": 10,
	        "max_score": null,
	        "hits": [
	            {
	            	"_index": "test_search_20210628",
	                "_type": "profiles",
	                "_id": "3692613",
	                "_score": null,
	                "_source": {
	                    "username": "test_user",
	                    "language": "en",
	                    "age": 32,
	                    "height": "157"
	                },
	                "sort": [
	                    1637620268000
	                ]
	            }
	            ...
	        ]
	    }
	}
}
*/
```

## Accessing Elasticsearch Client

If ever needed you can also access the client directly

```js
import es from "Lesgo/Utils/elasticsearch";

const client = es().getClient();

await client.index({
  index: "game-of-thrones",
  body: {
    character: "Ned Stark",
    quote: "Winter is coming.",
  },
});
```

## Available methods

### es.setConnection

This will set the current connection to either `aws` or `default`.

```js
import es from "Lesgo/Utils/elasticsearch";

es().setConnection("aws");
```

### es.createIndices

This will create a new index.

```js
import es from "Lesgo/Utils/elasticsearch";

await es().createIndices(
  {
    character: "Tyrion Lannister",
    quote: "A mind needs books like a sword needs a whetstone.",
  },
  "game-of-thrones"
);
```

### es.deleteIndices

This will delete one or more indices. For more options, refer [here](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference.html#_delete)

```js
import es from "Lesgo/Utils/elasticsearch";

await es().deleteIndices(["game-of-thrones", "twitter", "store"]);

// More options
await es().deleteIndices("game-of-thrones", {
  ignore_unavailable: true,
  timeout: "2000",
});
```

### es.existIndices

This will check if an index or indices exists. For more options, refer [here](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference.html#_exists)

```js
import es from "Lesgo/Utils/elasticsearch";

await es().existIndices("game-of-thrones");

// More options
await es().existIndices("game-of-thrones", {
  id: "1",
});
```

### es.putMapping

This will add new fields to an existing data stream or index.

!!! warning "Warning"

    Due to deprecation of `type` from Elasticsearch 7.0.0, use `es.getClient()` directly. For more info, refer [here](https://www.elastic.co/guide/en/elasticsearch/reference/7.16/removal-of-types.html)

```js
import es from "Lesgo/Utils/elasticsearch";

await es().putMapping("twitter", "user", {
  name: { type: "text" },
  user_name: { type: "keyword" },
  email: { type: "keyword" },
});
```

### es.get

This will retrieve the specified JSON document from an index using the document ID.

```js
import es from "Lesgo/Utils/elasticsearch";

await es({ index: "game-of-thrones" }).get("0");

/**
{
    "_index":"game-of-thrones",
    "_type":"_doc",
    "_id":"0",
    "_version":1,
    "_seq_no":0,
    "_primary_term":1,
    "found":true,
    "_source":{
        "@timestamp":"2099-11-15T14:12:12",
        "http":{
            "request":{
                "method":"get"
            },
            "response":{
                "status_code":200,
                "bytes":1070000
            },
            "version":"1.1"
        },
        "source":{
            "ip":"127.0.0.1"
        },
        "message":"GET /search HTTP/1.1 200 1070000",
        "user":{
            "id":"kimchy"
        }
    }
}
*/
```

### es.search

This will return documents based on search query.

```js
es.search(
  body: Object, // query object to execute
);
```

**Example Usage**

```js
import es from "Lesgo/Utils/elasticsearch";

const search = es({ index: "game-of-thrones" });

await search.search({
  query: {
    match: {
      "user.id": "kimchy",
    },
  },
});
```

### es.msearch

This will execute multiple different searches using a single API request.

```js
es.msearch(
  body: Array, // array of query objects to execute
);
```

**Example Usage**

```js
import es from "Lesgo/Utils/elasticsearch";

const search = es({ index: "game-of-thrones" });

await search.msearch([
  { index: "game-of-thrones" },
  {
    query: {
      bool: {
        must: [
          {
            match: {
              hashtag: "new",
            },
          },
        ],
        must_not: [
          {
            match: {
              hashtag: "old",
            },
          },
        ],
      },
    },
  },
  { index: "game-of-thrones" },
  {
    query: {
      bool: {
        must: [
          {
            match: {
              hashtag: "new",
            },
          },
        ],
        must_not: [
          {
            match: {
              hashtag: "old",
            },
          },
        ],
      },
    },
  },
]);
```

Learn more with [Multi Search API](https://www.elastic.co/guide/en/elasticsearch/reference/7.13/search-multi-search.html).

### es.indexOrCreateById / es.create

This will add a JSON document to the specified data stream or index and makes it searchable. If the target is an index and the document already exists, the request updates the document and increments its version, else it is created.

```js
import es from "Lesgo/Utils/elasticsearch";

await es({ index: "game-of-thrones" }).create("1", {
  character: "Tyrion Lannister",
  quote: "A mind needs books like a sword needs a whetstone.",
});

// Third parameter is to refresh or not
await es({ index: "game-of-thrones" }).indexOrCreateById(
  {
    id: "1",
    character: "Tyrion Lannister",
    quote: "A mind needs books like a sword needs a whetstone.",
  },
  true
);

/**
{
    "_shards":{
        "total":2,
        "failed":0,
        "successful":2
    },
    "_index":"game-of-thrones",
    "_type":"_doc",
    "_id":"W0tpsmIBdwcYyG50zbta",
    "_version":1,
    "_seq_no":0,
    "_primary_term":1,
    "result":"created"
}
*/
```

### es.bulkIndex

This will perform multiple indexing or delete operations in a single API call. This reduces overhead and can greatly increase indexing speed.

```js
import es from "Lesgo/Utils/elasticsearch";

await es({ index: "game-of-thrones" }).bulkIndex([
  {
    profile_id: "1",
    text: "If I fall, don't bring me back.",
    user: "jon",
    date: new Date(),
  },
  {
    profile_id: "2",
    text: "Winter is coming",
    user: "ned",
    date: new Date(),
  },
]);

/**
{
    "took":30,
    "errors":false,
    "items":[
        {
            "index":{
                "_index":"game-of-thrones",
                "_type":"_doc",
                "_id":"1",
                "_version":1,
                "result":"created",
                "_shards":{
                    "total":4,
                    "successful":2,
                    "failed":0
                },
                "status":201,
                "_seq_no":0,
                "_primary_term":1
            }
        }
    ]
}
*/
```

### es.create

This adds a JSON document to the specified data stream or index and makes it searchable.

```js
import es from "Lesgo/Utils/elasticsearch";

await es({ index: "game-of-thrones" }).create("1", {
  text: "If I fall, don't bring me back.",
  user: "jon",
  date: new Date(),
});

/**
{
    "_shards":{
        "total":2,
        "failed":0,
        "successful":2
    },
    "_index":"game-of-thrones",
    "_type":"_doc",
    "_id":"1",
    "_version":1,
    "_seq_no":0,
    "_primary_term":1,
    "result":"created"
}
*/
```
