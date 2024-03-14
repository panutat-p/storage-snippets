# Bulk

https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-bulk.html

## Bulk Insert

```sh
POST /fruits/_bulk
{ "index" : { "_id" : "1" } }
{ "name": "apple", "color": "red", "price": 15,"tag": ["fruit"], "created_at": "2020-12-31" }
{ "index" : { "_id" : "2" } }
{ "name": "banana","color": "yellow", "price": 8,"tag": ["fruit"], "created_at": "2021-12-31" }
{ "index" : { "_id" : "3" } }
{ "name": "carrot","color": "orange", "price": 10,"tag": ["fruit"], "created_at": "2022-12-31" }
{ "index" : { "_id" : "4" } }
{ "name": "durian","color": "green", "price": 100,"tag": ["fruit","premium"], "created_at": "2023-12-31" }
{ "index" : { "_id" : "5" } }
{ "name": "green grapes","color": "green", "price": 35,"tag": ["fruit"], "created_at": "2024-12-31" }
{ "index" : { "_id" : "6" } }
{ "name": "red grapes","color": "red", "price": 25,"tag": ["fruit"], "created_at": "2025-12-31" }
{ "index" : { "_id" : "7" } }
{ "name": "hazelnut","color": "brown", "price": 18,"tag": ["bean"], "created_at": "2026-12-31" }
{ "index" : { "_id" : "8" } }
{ "name": "hazelnut old","color": "brown", "price": 18,"tag": ["bean"], "created_at": "2020-12-31" }
```

## Bulk Update

```sh
POST /fruits/_bulk
{ "update" : { "_id" : "1" } }
{ "doc" : {"price":10} }
{ "update" : { "_id" : "2" } }
{ "doc" : {"price":12} }
```

## Bulk Upsert

```sh
POST /fruits/_bulk
{ "update" : { "_id" : "1" } }
{ "doc" : {"color": "red", "price": 15}, "doc_as_upsert" : true }
{ "update" : { "_id" : "2" } }
{ "doc" : {"color": "yellow", "price": 8}, "doc_as_upsert" : true }
```
