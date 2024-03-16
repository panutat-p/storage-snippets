# Aggregation

https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html

https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-terms-aggregation.html

## Document count error

| Parameter | Description |
|---|---|
| `doc_count_error_upper_bound` | Gives an upper bound on the error in the document count in the bucket results. It's not an absolute error, but a worst-case scenario. This means that the actual error is usually lower. The value is only provided when the size parameter is used. |
| `sum_other_doc_count` | Represents the total count of all documents that did not make it into the top buckets (as defined by the size parameter). This can give you an idea of how many documents you're missing out on in your aggregation. |

| Parameter | Description |
|---|---|
| `size` | Limit the number of top terms returned by the terms aggregation. If you set size to 10, Elasticsearch will return the top 10 terms ordered by the document count. |
| `shard_size` | Determines how many terms the coordinating node will request from each shard. A higher shard_size will improve the accuracy of the terms aggregation at the cost of using more memory. This is because a larger shard_size means more terms are returned from each shard to the coordinating node,  which then has more terms to work with when calculating the final top terms. |

## Simple

```sh
GET /fruits/_search
{
  "size": 0,
  "aggs": {
    "colors": {
      "terms": {
        "size": 10,
        "field": "color"
      }
    }
  }
}
```

## Filter then aggregation

```sh
GET /fruits/_search
{
  "size": 0,
  "query": {
    "bool": {
      "must": [
        {
          "range": {
            "price": { "gte": 10 }
          }
        }
      ]
    }
  },
  "aggs": {
    "color_group": {
      "terms": {
        "size": 10,
        "field": "color"
      }
    }
  }
}
```

## Sub-aggregation

### Average price of each color

```sh
GET /fruits/_search
{
  "size": 0,
  "aggs": {
    "color_group": {
      "terms": { "size": 10, "field": "color" },
      "aggs": {
        "avg_price": {
          "avg": { "field": "price" }
        }
      }
    }
  }
}
```

### Count price >= 10 of each color

```sh
GET /fruits/_search
{
  "size": 0,
  "aggs": {
    "colors": {
      "terms": { "size": 10, "field": "color" },
      "aggs": {
        "gte_10": {
          "filter": { "range": {"price": {"gte": 10} }}
        }
      }
    }
  }
}
```

### Count premium fruits of each color

```sh
GET /fruits/_search
{
  "size": 0,
  "aggs": {
    "colors": {
      "terms": { "size": 10, "field": "color" },
      "aggs": {
        "premium_fruits": {
          "filter": { "term": {"tag": "premium" } }
        }
      }
    }
  }
}
```

```sh
GET /fruits/_search
{
  "size": 0,
  "aggs": {
    "colors": {
      "terms": { "size": 10, "field": "color" },
      "aggs": {
        "premium_fruits": {
          "filter": {
            "bool": {
              "must": [
                { "term": {"tag": "premium" } }
              ]
            }
          }
        }
      }
    }
  }
}
```
