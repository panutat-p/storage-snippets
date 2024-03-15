# Aggregation

https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html

https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-terms-aggregation.html

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