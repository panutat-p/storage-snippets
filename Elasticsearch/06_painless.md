# Painless Script

## Performance

https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-scripting-painless.html

* Painless compiles directly into JVM bytecode
* Painless typically avoids features that require additional slower checks at runtime

https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-scripting-using.html

* The first time Elasticsearch sees a new script, it compiles the script and stores the compiled version in a cache
* Compilation can be a heavy process
* You can compile up to 150 scripts per 5 minutes by default
* For ingest contexts, the default script compilation rate is unlimited
* If you compile too many unique scripts within a short time, Elasticsearch rejects the new dynamic scripts with a `circuit_breaking_exception` error

https://www.elastic.co/guide/en/elasticsearch/reference/current/scripts-and-search-speed.html

* Script cannot use Elasticsearch’s index structures or related optimizations
* Result in slower search speeds.

## `now`

https://www.elastic.co/guide/en/elasticsearch/painless/current/painless-datetime.html#_datetime_now

* Most Painless contexts, `now` is not supported
* scripts are often run once per document, so each time the script is run a different `now` is returned
* scripts are often run in a distributed fashion without a way to appropriately synchronize `now`

## Script field

```sh
GET /fruits/_search
{
  "script_fields" : {
    "expired_at" : {
      "script" : {
        "lang": "painless",
        "source": "doc['created_at'].value.plusDays(30)"
      }
    }
  }
}
```

## Update documents in range

```sh
POST /fruits/_update_by_query
{
  "script": {
    "lang": "painless",
    "source": "ctx._source.tags.add('premium')"
  },
  "query": {
    "range": {
      "price": { "gt": 100 }
    }
  }
}
```

## Sort context

https://www.elastic.co/guide/en/elasticsearch/painless/current/painless-sort-context.html

```sh
GET /fruits/_search
{
  "sort": {
    "_script": {
      "type": "number",
      "order": "asc",
      "script": {
        "lang": "painless",
        "source": """
          if (doc['color'].empty) return 99;
          String color = doc['color'].value;
          if (color == 'red') return 1;
          if (color == 'orange') return 2;
          if (color == 'yellow') return 3;
          if (color == 'blue') return 4;
          if (color == 'green') return 5;
          return 6;
        """
      }
    }
  }
}
```
