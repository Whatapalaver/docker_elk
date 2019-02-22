Basic ElasticSearch Aggregations
---

Following tutorial - [An Introduction to ES Aggregations](https://qbox.io/blog/elasticsearch-aggregations)

The extracts below have been updated for version 6 of ES.

- Create Mapping

```
curl -XPUT "http://localhost:9200/sports/" -d'
{
   "mappings": {
      "athlete": {
         "properties": {
            "birthdate": {
               "type": "date",
               "format": "dateOptionalTime"
            },
            "location": {
               "type": "geo_point"
            },
            "name": {
               "type": "keyword"
            },
            "rating": {
               "type": "integer"
            },
            "sport": {
               "type": "keyword"
            }
         }
      }
   }
}'
```

- Bulk Posting

```
curl -XPOST "http://localhost:9200/sports/_bulk" -d'
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Michael", "birthdate":"1989-10-1", "sport":"Baseball", "rating": ["5", "4"],  "location":"46.22,-68.45"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Bob", "birthdate":"1989-11-2", "sport":"Baseball", "rating": ["3", "4"],  "location":"45.21,-68.35"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Jim", "birthdate":"1988-10-3", "sport":"Baseball", "rating": ["3", "2"],  "location":"45.16,-63.58" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Joe", "birthdate":"1992-5-20", "sport":"Baseball", "rating": ["4", "3"],  "location":"45.22,-68.53"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Tim", "birthdate":"1992-2-28", "sport":"Baseball", "rating": ["3", "3"],  "location":"46.22,-68.85"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Alfred", "birthdate":"1990-9-9", "sport":"Baseball", "rating": ["2", "2"],  "location":"45.12,-68.35"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Jeff", "birthdate":"1990-4-1", "sport":"Baseball", "rating": ["2", "3"], "location":"46.12,-68.55"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Will", "birthdate":"1988-3-1", "sport":"Baseball", "rating": ["4", "4"], "location":"46.25,-68.55" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Mick", "birthdate":"1989-10-1", "sport":"Baseball", "rating": ["3", "4"],  "location":"46.22,-68.45"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Pong", "birthdate":"1989-11-2", "sport":"Baseball", "rating": ["1", "3"],  "location":"45.21,-68.35"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Ray", "birthdate":"1988-10-3", "sport":"Baseball", "rating": ["2", "2"],  "location":"45.16,-63.58" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Ping", "birthdate":"1992-5-20", "sport":"Baseball", "rating": ["4", "3"],  "location":"45.22,-68.53"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Duke", "birthdate":"1992-2-28", "sport":"Baseball", "rating": ["5", "2"],  "location":"46.22,-68.85"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Hal", "birthdate":"1990-9-9", "sport":"Baseball", "rating": ["4", "2"],  "location":"45.12,-68.35"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Charge", "birthdate":"1990-4-1", "sport":"Baseball", "rating": ["3", "2"], "location":"46.12,-68.55"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Barry", "birthdate":"1988-3-1", "sport":"Baseball", "rating": ["5", "2"], "location":"46.25,-68.55" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Bank", "birthdate":"1988-3-1", "sport":"Golf", "rating": ["6", "4"], "location":"46.25,-68.55" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Bingo", "birthdate":"1988-3-1", "sport":"Golf", "rating": ["10", "7"], "location":"46.25,-68.55" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"James", "birthdate":"1988-3-1", "sport":"Basketball", "rating": ["10", "8"], "location":"46.25,-68.55" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Wayne", "birthdate":"1988-3-1", "sport":"Hockey", "rating": ["10", "10"], "location":"46.25,-68.55" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Brady", "birthdate":"1988-3-1", "sport":"Football", "rating": ["10", "10"], "location":"46.25,-68.55" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Lewis", "birthdate":"1988-3-1", "sport":"Football", "rating": ["10", "10"], "location":"46.25,-68.55" }
'
```

Example Aggregation Query
===

```
curl -XPOST "http://localhost:9200/sports/athlete/_search" -d'
{
   "size": 0,
   "aggregations": {
      "sport": {
         "terms": {
            "field": "sport"
         }
      }
   }
}'
```

Further ES Bucket Aggregations
===

Following at tutorial at [iridakos.com](https://iridakos.com/tutorials/2018/10/22/elasticsearch-bucket-aggregations.html)

NB. This is a useful tutorial and well worth reading in it's original format.

Create index:

```
PUT city_offices
{
  "settings": {
    "number_of_shards": 1
  },
  "mappings": {
    "_doc": {
      "properties": {
        "city": {
          "type": "keyword"
        },
        "office_type": {
          "type": "keyword"
        },
        "citizens": {
          "type": "nested",
          "properties": {
            "occupation": {
              "type": "keyword"
            },
            "age": {
              "type": "integer"
            },
            "pets": {
              "type": "nested",
              "properties": {
                "kind": {
                  "type": "keyword"
                  },
                "name": {
                  "type": "keyword"
                },
                "age": {
                  "type": "integer"
                }
              }
            }
          }
        }
      }
    }
  }
}
```

Run bulk import from sample data 'iridakos-sample.json' or from [randomly generated output](https://github.com/iridakos/iridakos-posts/blob/master/2018-09-22-elasticsearch-bucket-aggregations/generate-random-data.rb) using a cool tool.


```
curl -s -H "Content-Type: application/x-ndjson" -XPOST http://localhost:9200/_bulk --data-binary "@iridakos-sample.json"; echo
```

Initial Aggregation Query:

```
GET city_offices/_search
{
  "aggs": {
    "cities": {
      "terms": { 
        "field": "city",
        "size": 50
      }
    }
  }
}
```

Note that this search has the 'hits' attribute with all results from the query. You could remove these with `"size": 0,`
The 'aggregations' attribute holds the bit we are interested in.
The default size for aggregates is 10 unless specified as something else. Here it has been overridden to 50 which ensues that `sum_other_doc_count` is zero

Adding multiple aggregations:

```
GET city_offices/_search
{
  "size": 0,
  "aggs": {
    "cities": {
      "terms": { 
        "field": "city",
        "size": 50
        
      }
    },
    "office_types": {
      "terms": {
        "field": "office_type"
      }
    }
  }
}
```

Search and Aggregation
===

If you add a search query, the aggregations will only apply to the search results.

```
GET city_offices/_search
{
  "size": 0,
  "query": {
    "term": {
      "city": {
        "value": "Athens"
      }
    }
  },
  "aggs": {
    "cities": {
      "terms": {
        "field": "city",
        "size": 50
      }
    },
    "office_types": {
      "terms": {
        "field": "office_type",
        "size": 10
      }
    }
  }
}
```