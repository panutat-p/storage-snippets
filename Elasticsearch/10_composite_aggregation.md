# Aggregation

https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-composite-aggregation.html


## Size

https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-composite-aggregation.html#_size


## Pagination

https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-composite-aggregation.html#_pagination


```jsonl
GET /fruits/_search
{
  "size": 0,
  "aggs": {
    "colors": {
      "composite": {
        "size": 10,
        "sources": [
          {
            "color": { "terms": { "field": "color" } }
          }
        ]
      }
    }
  }
}
```

```json lines
GET /fruits/_search
{
  "size": 0,
  "aggs": {
    "colors": {
      "composite": {
        "size": 10,
        "after": { "color": "yellow" },
        "sources": [
          {
            "color": { "terms": { "field": "color" } }
          }
        ]
      }
    }
  }
}
```
