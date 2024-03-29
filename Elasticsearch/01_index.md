# Index

## List indices

https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-indices.html

```shell
GET /_cat/indices?v=true&s=index
```

```shell
GET /_cat/indices/fruits*?v=true&s=index
```

## Create `fruits` index

https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html

```sh
DELETE /fruits
```

```sh
PUT /fruits
{
  "settings": {
    "index": {
      "number_of_shards": 1,
      "number_of_replicas": 0
    }
  },
  "mappings": {
  "dynamic": false,
    "properties": {
      "name": {
        "type": "text"
      },
      "color": {
        "type": "keyword"
      },
      "price": {
        "type": "scaled_float",
        "scaling_factor": 100
      },
      "tag": {
        "type": "keyword"
      },
      "created_at": {
        "type": "date",
        "format":"yyyy-MM-dd"
      }
    }
  }
}
```

## Update `fruits` index

```sh
PUT /fruits/_mapping
{
  "properties": {
    "grade": {
      "type": "integer"
    }
  }
}
```

## `_id`

> Loading the fielddata on the _id field is deprecated and will be removed in future versions.
> If you require sorting or aggregating on this field you should also include the id in the body of your documents,
> and map this field as a keyword field that has `doc_values` enabled

## `doc_values`

https://www.elastic.co/guide/en/elasticsearch/reference/current/doc-values.html

> All fields which support doc values have them enabled by default.
> If you are sure that you don’t need to sort or aggregate on a field, or access the field value from a script,
> you can disable doc values in order to save disk space

```sh
PUT /fruit_signatures
{
  "settings": {
    "index": {
      "number_of_shards": 1,
      "number_of_replicas": 0
    }
  },
  "mappings": {
  "dynamic": false,
    "properties": {
      "name": {
        "type": "keyword"
        "doc_values": true
      },
      "hash": {
        "type": "keyword"
        "doc_values": false
      }
    }
  }
}
```
