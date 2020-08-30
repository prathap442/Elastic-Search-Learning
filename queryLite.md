> What is the Query Lite Interface?
The query lite interface is the way interacting through the elasticsearch by means query and the parameters exist in the query itself .


```
  curl -XGET http://127.0.0.1:9202/movies/_search\?q\=title:Interstellar+year:2014\&pretty\=true

{
  "took" : 4,
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
    "max_score" : 0.6931471,
    "hits" : [
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "10499",
        "_score" : 0.6931471,
        "_source" : {
          "title" : "Interstellar",
          "rating" : 32
        }
      }
    ]
  }
} 
```

Here if we try to look at the query the query has got some query parameters and the query parameters are dealing with the query parameters .

Usage of the Wildcards in the elastic search
```
GET /movies/_search?pretty
{
  "query": {
    "wildcard": {
      "title": {
        "value": "Inter*"
      }
    }
  }
}
```

The query can be written with bool and the query can be something of this sort .
```
GET /movies/_search?pretty
{
  "query": {
    "bool": {
      "must": { "term": { "title": "Interstellar"} },
      "filter": { "range": { "rating": {"gte": 32}}}
    }
  }
}
```


bool is used for checking the conditions
a query is used for writing the queries 
```
GET /movies/_search?pretty
{
  "query": {
    "bool": {
      "must": { "term": { "title": "Interstellar"} },
      "filter": { "range": { "rating": {"gte": 32}}}
    }
  }
}
```

Usage of the terms filter to match and see if there are any matches atleast one in the possible set 
```
GET /movies/_search?pretty
{
  "query": {
    "bool": {
      "must": { "term": { "genre": ["sci-fi", "fiction"]} },
      "filter": { "range": { "rating": {"gte": 32}}}
    }
  }
}
```



> Match Phrase Usage
```
GET /movies/_search?pretty
{
  "query": {
    "match_phrase": {
      "title": "Loafer"
    }
  }
}

```

Usage of the Match Phrase to match the keywords

> if the no of keywords gap between two words if it is one 
```
GET /series/_search?pretty
{
  "query": {
    "match_phrase": {
      "title": {"query": "child1 ghost", "slop": 1}
    }
  }  
}

The below would be the result for this 

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
    "max_score" : 1.2039728,
    "hits" : [
      {
        "_index" : "series",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 1.2039728,
        "_source" : {
          "title" : "child1 fragment ghost",
          "name" : "film",
          "parent" : 1
        }
      }
    ]
  }
}
```


> Usage of the Must keyword 
```
GET /series/_search?pretty
{
  "query": {
    "bool": {
      "must": [
        {
          "match_phrase": {
            "title": {
              "query": "child1", "slop": 1
            }
          }
        }
      ]
    }
  }  
}
```


> Pagination in Elasticsearch
```
  curl -XGET http://localhost:9202 -d '
GET /series/_search?pretty
  {
   "from": 0,
   "size": 2,
   "query": {
     "bool": {
       "must": [
         {"match_phrase": {
           "title": {
             "query": "film", "slop":1
           }
         }}
       ]
     }
   }
}
  '


>output
{
  "took" : 2,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : 0.10536051,
    "hits" : [
      {
        "_index" : "series",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 0.10536051,
        "_source" : {
          "title" : "Parent1 film",
          "name" : "franchise"
        }
      },
      {
        "_index" : "series",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 0.10536051,
        "_source" : {
          "title" : "child1 film",
          "name" : "film",
          "parent" : 1
        }
      }
    ]
  }
}



```


> Sort

Generally when we want to sort then
```
  $ GET /movies/_search?pretty&sort=rating


  output:
  

  {
  "took" : 5,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 5,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "10487",
        "_score" : null,
        "_source" : {
          "title" : "Loafer",
          "rating" : 23
        },
        "sort" : [
          23
        ]
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "10499",
        "_score" : null,
        "_source" : {
          "title" : "Interstellar",
          "rating" : 32
        },
        "sort" : [
          32
        ]
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : null,
        "_source" : {
          "query" : {
            "title" : "Star War The beginning 1",
            "rating" : 2
          }
        },
        "sort" : [
          9223372036854775807
        ]
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "3",
        "_score" : null,
        "_source" : {
          "query" : {
            "title" : "Star War Goal The beginning 3",
            "rating" : 3
          }
        },
        "sort" : [
          9223372036854775807
        ]
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "5",
        "_score" : null,
        "_source" : {
          "query" : {
            "title" : "Modi Ranga Nayakulu",
            "rating" : 3
          }
        },
        "sort" : [
          9223372036854775807
        ]
      }
    ]
  }
}

```



If we wanted to do the searching on top of the text field that is analyzed then we cannot do that because?
```
   because the keywords are of the type text and they are being stored using the inverted index mechanism so that there is no scope in sorting . To have workaround for such things we need to drop the index and create newly by 


   curl -XPUT http://127.0.0.1:9200/movies -d '
   {
       "mappings": {
           "properties": {
             "title": {
                 "type": "text",
                 "fields": {
                     "raw": {  # by this we are creating a subfield in the main field of the title and can be accessed when required by title.raw
                         "type": "keyword"
                     }
                 }
             }     
           }
       }
   }
   '
```


And an example for this could be 
```
$ GET /movies/_search?pretty&sort=title.raw


{
  "took" : 208,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : null,
        "_source" : {
          "title" : "star-1",
          "description" : "this is the description1",
          "rating" : 1
        },
        "sort" : [
          "star-1"
        ]
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : null,
        "_source" : {
          "title" : "star-2",
          "description" : "this is the description2",
          "rating" : 23
        },
        "sort" : [
          "star-2"
        ]
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "3",
        "_score" : null,
        "_source" : {
          "title" : "star-2",
          "description" : "this is the description3",
          "rating" : 45
        },
        "sort" : [
          "star-2"
        ]
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "4",
        "_score" : null,
        "_source" : {
          "title" : "star-4",
          "description" : "this is the description4",
          "rating" : 45
        },
        "sort" : [
          "star-4"
        ]
      }
    ]
  }
}

```



> Something do on my own
```
GET /movies/_search?pretty
{
  "query": {
    "bool": {
      "must": [
        {"match_phrase": {"title": {
          "query": "star"
        }}
        }
      ],
      "must_not": [
        {"match": {
          "rating": "23"
        }}
      ],
      "filter": [
        {"range": {
          "rating": {
            "gte": 10,
            "lte": 90
          }
        }}
      ]
    }
  }
}

output:

{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 2,
      "relation" : "eq"
    },
    "max_score" : 0.05715841,
    "hits" : [
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "3",
        "_score" : 0.05715841,
        "_source" : {
          "title" : "star-2",
          "description" : "this is the description3",
          "rating" : 45
        }
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "4",
        "_score" : 0.05715841,
        "_source" : {
          "title" : "star-4",
          "description" : "this is the description4",
          "rating" : 45
        }
      }
    ]
  }
}

```


>>> Fuzziness In Queries

- This can be used to check the searching based on the spell mistakes
like intrastellar would fetch the movie interstellar 

```
GET movies/_search?pretty
{
      "query": {
          "fuzzy": {
              "title": {
                  "value": "star2",
                  "fuzziness": 1
              }
          }
      }
}


-----------------
output:

{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : 0.042868808,
    "hits" : [
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 0.042868808,
        "_source" : {
          "title" : "star-1",
          "description" : "this is the description1",
          "rating" : 1
        }
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 0.042868808,
        "_source" : {
          "title" : "star-2",
          "description" : "this is the description2",
          "rating" : 23
        }
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "3",
        "_score" : 0.042868808,
        "_source" : {
          "title" : "star-2",
          "description" : "this is the description3",
          "rating" : 45
        }
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "4",
        "_score" : 0.042868808,
        "_source" : {
          "title" : "star-4",
          "description" : "this is the description4",
          "rating" : 45
        }
      }
    ]
  }
}
```


>>> Learning on PArtial Matching in Elastic Search
Ans: Some sort of the prefix or the subset matching based queries can be used in the elasticsearch do come under the partial matchings .
What are prefix queries?
  - A prefix query is the one where intial set of the letter for a word would match .
  Example:
  ```
    GET /movies/_search?pretty
    {
        "query": {
            "prefix": {
                "title": "star"
            }
        }
    }

    
    output:
{
  "took" : 15,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "title" : "star-1",
          "description" : "this is the description1",
          "rating" : 1
        }
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 1.0,
        "_source" : {
          "title" : "star-2",
          "description" : "this is the description2",
          "rating" : 23
        }
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "3",
        "_score" : 1.0,
        "_source" : {
          "title" : "star-2",
          "description" : "this is the description3",
          "rating" : 45
        }
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "4",
        "_score" : 1.0,
        "_source" : {
          "title" : "star-4",
          "description" : "this is the description4",
          "rating" : 45
        }
      }
    ]
  }
}



  ```
     

What is a wildcard query?
- We can even use a wildcard as an expression for the query that we do and for the sake of the matching of the title with that query
Example:
```
    GET /movies/_search?pretty
    {
        "query": {
            "wildcard": {
                "title": "star*"
            }
        }
    }
  
  --------------------------

    {
    "took" : 2,
    "timed_out" : false,
    "_shards" : {
        "total" : 1,
        "successful" : 1,
        "skipped" : 0,
        "failed" : 0
    },
    "hits" : {
        "total" : {
        "value" : 4,
        "relation" : "eq"
        },
        "max_score" : 1.0,
        "hits" : [
        {
            "_index" : "movies",
            "_type" : "_doc",
            "_id" : "1",
            "_score" : 1.0,
            "_source" : {
            "title" : "star-1",
            "description" : "this is the description1",
            "rating" : 1
            }
        },
        {
            "_index" : "movies",
            "_type" : "_doc",
            "_id" : "2",
            "_score" : 1.0,
            "_source" : {
            "title" : "star-2",
            "description" : "this is the description2",
            "rating" : 23
            }
        },
        {
            "_index" : "movies",
            "_type" : "_doc",
            "_id" : "3",
            "_score" : 1.0,
            "_source" : {
            "title" : "star-2",
            "description" : "this is the description3",
            "rating" : 45
            }
        },
        {
            "_index" : "movies",
            "_type" : "_doc",
            "_id" : "4",
            "_score" : 1.0,
            "_source" : {
            "title" : "star-4",
            "description" : "this is the description4",
            "rating" : 45
            }
        }
        ]
    }
    }




```
What is a reqexp query?

- A regexp based query is the one where the querying could be done based on the regural expression in the query
```
    GET /movies/_search?pretty
    {
        "query": {
            "regexp": {
                "title": "s.*r"
            }
        }
    }


    output:
    {
    "took" : 11,
    "timed_out" : false,
    "_shards" : {
        "total" : 1,
        "successful" : 1,
        "skipped" : 0,
        "failed" : 0
    },
    "hits" : {
        "total" : {
        "value" : 4,
        "relation" : "eq"
        },
        "max_score" : 1.0,
        "hits" : [
        {
            "_index" : "movies",
            "_type" : "_doc",
            "_id" : "1",
            "_score" : 1.0,
            "_source" : {
            "title" : "star-1",
            "description" : "this is the description1",
            "rating" : 1
            }
        },
        {
            "_index" : "movies",
            "_type" : "_doc",
            "_id" : "2",
            "_score" : 1.0,
            "_source" : {
            "title" : "star-2",
            "description" : "this is the description2",
            "rating" : 23
            }
        },
        {
            "_index" : "movies",
            "_type" : "_doc",
            "_id" : "3",
            "_score" : 1.0,
            "_source" : {
            "title" : "star-2",
            "description" : "this is the description3",
            "rating" : 45
            }
        },
        {
            "_index" : "movies",
            "_type" : "_doc",
            "_id" : "4",
            "_score" : 1.0,
            "_source" : {
            "title" : "star-4",
            "description" : "this is the description4",
            "rating" : 45
            }
        }
        ]
  }
}

```


> Search As you Type?
- So when the people do type in the text we want the immediate fetch of the results by going through the index an fetching the results as quickly as possibly
```
    GET /movies/_search?pretty
    {
        "query": {
            "match_phrase_prefix": {
              "description": "is"
            }
        }
    }


------output----
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
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : 0.05715841,
    "hits" : [
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 0.05715841,
        "_source" : {
          "title" : "star-1",
          "description" : "this is the description1",
          "rating" : 1
        }
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 0.05715841,
        "_source" : {
          "title" : "star-2",
          "description" : "this is the description2",
          "rating" : 23
        }
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "3",
        "_score" : 0.05715841,
        "_source" : {
          "title" : "star-2",
          "description" : "this is the description3",
          "rating" : 45
        }
      },
      {
        "_index" : "movies",
        "_type" : "_doc",
        "_id" : "4",
        "_score" : 0.05715841,
        "_source" : {
          "title" : "star-4",
          "description" : "this is the description4",
          "rating" : 45
        }
      }
    ]
  }
}

```
