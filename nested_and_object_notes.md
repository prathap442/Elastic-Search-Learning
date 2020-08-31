```
PUT /developer
{
  "mappings": {
    "properties": {
      "first_name": {
        "type": "text"
      },
      "skills": {
        "type": "nested",
        "properties": 
        {
          "language": {
            "type": "keyword"
          },
          "level": {
            "type": "keyword"
          }
        }
      }
    }
  }
}


DELETE /developer


PUT /developer/_doc/1 
{
  "first_name": "chella prashant",
  "skills": [
    {
      "language": "ruby", 
      "level": "beginner"
    },
    {
      "language": "python",
      "level": "expert"
    }
  ]
}



PUT /developer/_doc/2
{
  "first_name": "Modi Ranga Nayak",
  "skills": [
    {
      "language": "ruby", 
      "level": "expert"
    },
    {
      "language": "python",
      "level": "expert"
    }
  ]
}





# to write the comments use #
GET developer/_search?pretty
{
  "query": {
    "nested": {
      "path": "skills",
      "query": {
        "bool": {
          "filter": [
            {
              "match": {
                "skills.language": "ruby"
              }
            },
            {
              "match": {
                "skills.level": "expert"
              }
            }
          ]
        }
      }
    }
  }
}




```


Now if i make the request that the developer should have the language asset ruby and the level to be "expert"

then i need to do the follwoing query in the nested way 
```
GET developer/_search?pretty
{
  "query": {
    "nested": {
      "path": "skills",
      "query": {
        "bool": {
          "filter": [
            {
              "match": {
                "skills.language": "ruby"
              }
            },
            {
              "match": {
                "skills.level": "expert"
              }
            }
          ]
        }
      }
    }
  }
}

```

# Output:
```
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 0.0,
    "hits" : [
      {
        "_index" : "developer",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 0.0,
        "_source" : {
          "first_name" : "Modi Ranga Nayak",
          "skills" : [
            {
              "language" : "ruby",
              "level" : "expert"
            },
            {
              "language" : "python",
              "level" : "expert"
            }
          ]
        }
      }
    ]
  }
}

```

