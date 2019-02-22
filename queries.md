List all Indices
===

`GET /_cat/indices?v`

Simplest Query All:

```
GET /_search
{
    "query": {
        "match_all": {}
    }
}
```

Query and Filter
===

Note that filters work on the cached query results, so are quick, but do not impact on the scoring and therefore relevancy of the return.

You can boost the relevancy score of a field using the carat ^

```
GET /courses/_search
{
  "query": {
    "bool": {
      "should": [
        { "match": {"room": "e3" } },
        { "range": {
          "students_enrolled": {
            "gte": 13,
            "lte": 14
          }
        }},
        {"multi_match": {
          "query": "market",
          "fields": ["name", "course_description^2"]
        }}
      ],
      "filter": {
        "bool": {
          "must": [
            { "range": {
              "students_enrolled": {
                "gte": 12
              }
            }}
          ]
        }
      }
    }
  }
}
```

Simple Query All
===

With pagination and sort.

```
GET /vehicles/cars/_search
{
  "from": 0,
  "size": 20,
  "query": {
    "match_all": {}
  },
  "sort": [
    {"price": {"order": "desc"}}
    ]
}
```

Count with Alternative Endpoints
===

So not just search.

```
GET /vehicles/cars/_count
{
  "query": {
    "match": {"make": "dodge"}
  }
}
```