# Clone

## Prepare

```shell
PUT /fruits_old
{
  "settings": {
    "index": {
      "number_of_shards": 1,  
      "number_of_replicas": 1
    }
  },
  "mappings":{
    "dynamic": false,
    "properties": {
      "name": {"type": "text"},
      "color": {"type": "keyword"},
      "price": {"type": "integer"},
      "tags": {"type": "keyword"}
    }
  }
}
```

```shell
POST /fruits_old/_bulk
{ "index" : { "_id" : "1" } }
{"name": "apple", "color": "red", "price": 15,"tags": ["fruit"]}
{ "index" : { "_id" : "2" } }
{"name": "banana","color": "yellow", "price": 8,"tags": ["fruit"]}
{ "index" : { "_id" : "3" } }
{"name": "carrot","color": "orange", "price": 10,"tags": ["fruit"]}
{ "index" : { "_id" : "4" } }
{"name": "durian","color": "green", "price": 100,"tags": ["fruit","premium"]}
{ "index" : { "_id" : "5" } }
```

## Block

```shell
PUT /fruits_old/_settings
{
  "index": {
    "blocks.read_only": true
  }
}
```

## Clone

```shell
POST /fruits_old/_clone/fruits_new
```

## Unblock

```shell
PUT /fruits_old/_settings
{
  "index": {
    "blocks.read_only": false
  }
}
```

```shell
PUT /fruits_new/_settings
{
  "index": {
    "blocks.read_only": false
  }
}
```

## Results

```shell
GET /fruits_old/_settings
```

```shell
GET /fruits_new/_settings
```

```shell
GET /fruits_old/_stats
```

```shell
GET /fruits_old/_stats
```

```shell
GET /fruits_old/_count
```

```shell
GET /fruits_new/_count
```

```shell
GET /fruits_old/_search
```

```shell
GET /fruits_new/_search
```

# Reindex

## Prepare

```shell
PUT /fruits_old
{
  "settings": {
    "index": {
      "number_of_shards": 1,  
      "number_of_replicas": 1
    }
  },
  "mappings":{
    "dynamic": false,
    "properties": {
      "name": {"type": "text"},
      "color": {"type": "keyword"},
      "price": {"type": "integer"},
      "tags": {"type": "keyword"}
    }
  }
}
```

```shell
POST /fruits_old/_bulk
{ "index" : { "_id" : "1" } }
{"name": "apple", "color": "red", "price": 15,"tags": ["fruit"]}
{ "index" : { "_id" : "2" } }
{"name": "banana","color": "yellow", "price": 8,"tags": ["fruit"]}
{ "index" : { "_id" : "3" } }
{"name": "carrot","color": "orange", "price": 10,"tags": ["fruit"]}
{ "index" : { "_id" : "4" } }
{"name": "durian","color": "green", "price": 100,"tags": ["fruit","premium"]}
{ "index" : { "_id" : "5" } }
```

## Create new index mapping

```shell
PUT /fruits_new
{
  "settings": {
    "index": {
      "number_of_shards": 1,  
      "number_of_replicas": 1
    }
  },
  "mappings":{
    "dynamic": false,
    "properties": {
      "name": {"type": "text"},
      "color": {"type": "text"},
      "price": {"type": "integer"},
      "tags": {"type": "text"}
    }
  }
}
```

## Reindex

```shell
POST _reindex
{
  "source": {
    "index": "fruits_old"
  },
  "dest": {
    "index": "fruits_new"
  }
}
```

## Results

```shell
GET /fruits_old/_settings
```

```shell
GET /fruits_new/_settings
```

```shell
GET /fruits_old/_stats
```

```shell
GET /fruits_old/_stats
```

```shell
GET /fruits_old/_search
```

```shell
GET /fruits_new/_search
```
