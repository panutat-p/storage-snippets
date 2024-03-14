# Mapping Migration

## Update missing fields

### synchronous

```sh
POST /fruits/_update_by_query?conflicts=proceed&slices=auto
{
  "max_docs": 10000,
  "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "grade"
        }
      }
    }
  },
  "script": {
    "lang": "painless",
    "source": "ctx._source.grade = 0"
  }
}
```

### asynchronous

```sh
POST /fruits/_update_by_query?conflicts=proceed&slices=auto&wait_for_completion=false
{
  "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "grade"
        }
      }
    }
  },
  "script": {
    "lang": "painless",
    "source": "ctx._source.grade = 0"
  }
}
```

```sh
GET /_tasks/$id
```

```sh
POST /_tasks/$id/_cancel
```

## Check results

```sh
GET /fruits/_count
{
  "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "grade"
        }
      }
    }
  }
}
```

```sh
GET /fruits/_count
{
  "query": {
    "bool": {
      "must": {
        "exists": {
          "field": "grade"
        }
      }
    }
  }
}
```
