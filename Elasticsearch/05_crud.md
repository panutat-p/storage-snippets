# CRUD

## Insert

https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html

```sh
POST /fruits/_doc/1
{
  "name": "apple",
  "color": "red",
  "price": "15",
  "tag": ["fruit"],
  "created_at": "2024-12-31"
}
```

## Update

https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-update.html

```sh
POST /fruits/_update/1
{
  "doc": {
    "price": "100"
  }
}
```

* `refresh=false` default
* `refresh=true` force Elasticsearch to refresh the relevant shard immediately
* `refresh=wait_for` wait for the next scheduled refresh

```sh
POST /fruits/_update/1?refresh=wait_for
{
  "doc": {
    "price": 100
  }
}
```

## Search

```sh
GET /fruits/_search?filter_path=hits.hits._id,hits.hits._source
{
  "size": 10,
  "_source": ["name", "price"],
  "sort": [
    { "name": { "order": "asc"} }
  ]
}
```

## `match`

```sh
GET /fruits/_search
{
  "query": {
    "match": {
      "name": "apple"
    }
  }
}
```

## `range`

```sh
GET /fruits/_search
{
  "query": {
    "range": {
      "expired_at": {"gt": "2024-03-07"}
    }
  }
}
```

```sh
GET /fruits/_search
{
  "query": {
    "range": {
      "expired_at": {"gt": "now/d"}
    }
  }
}
```

```sh
GET /fruits/_search
{
  "query": {
    "bool": {
      "must": [
        { "range": { "price": {"gte": 10} } }
      ]
    }
  }
}
```

## `filter`

```sh
GET /fruits/_search
{
  "query": {
    "bool": {
      "filter": [
        { "term": { "color": "red" } }
      ]
    }
  }
}
```

```sh
GET /fruits/_search
{
  "query": {
    "bool": {
      "must_not": [
        { "term": { "color": "red"} }
      ]
    }
  }
}
```

```sh
GET /fruits/_search
{
  "query": {
    "bool": {
      "filter": [
        { "terms": { "color": ["green", "red"] } }
      ]
    }
  }
}
```

## `sort`

```sh
GET /fruits/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "range": {
            "expired_at": {"gte": "2024-03-07"}
          }
        }
      ]
    }
  },
  "sort": [
    {
      "expired_at": {"order": "asc"}
    }
  ]
}
```

## Delete

```sh
DELETE /fruits/_doc/1
```

```sh
POST /fruits/_delete_by_query
{
  "query": {
    "match_all": {}
  }
}
```

```sh
POST /fruits/_delete_by_query
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {"name": "apple"}
        }
      ]
    }
  }
}
```
