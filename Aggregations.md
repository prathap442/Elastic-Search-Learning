Aggregations are a beautiful spots to hold up the data or collectionize the data and perform certain operations on the Aggregations .

- The Elastic Search Excels at some of the points in analyzing the data than the Hadoop and the Spark in terms of time taken to perform the operations .

- The Elastic Search uses these aggregations to show the analytics and analyze the analytics even .

- They are used for constructing the histograms .

- They are used for analyzing the datasets of timeseries even .

Now let us have a loot at some of those Aggregations .

> Rule of thumb
```
  When using the aggregations remember the body should have "aggs" as the key involved in it .

  curl -XGET http://localhost:9202/movies?pretty&size=0 -d 
  {
    # aggs is key that should be used when performing the aggregations  
    "aggs": {
       #here we will define the name of the aggregation that we wanted to design
       "movies_rating_aggregator": {
           # here avg would be function that is being given by the elastic search to perfomr the average operation
           "terms": {
               # on what field do we basically perform this .
               "field": "rating"
           }
       }
    } 
  }
```


