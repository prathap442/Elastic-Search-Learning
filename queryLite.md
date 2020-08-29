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