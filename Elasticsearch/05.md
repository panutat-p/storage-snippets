# Search

## wildcard

```sh
GET /fruits/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "wildcard": {
            "name": { "value": "*na*", "case_insensitive": true }
          }
        }
      ]
    }
  }
}
```

## function score

```sh
GET /fruits/_search
{
  "query": {
    "function_score": {
      "functions": [
        {
          "random_score": {
            "field": "_seq_no",
            "seed": "100"
          }
        }
      ]
    }
  }
}
```
