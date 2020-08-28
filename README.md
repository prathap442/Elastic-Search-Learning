What is document and the Index?


An Index as the table when coming to the analogy of mapping the document to the row in the table and the Index as the Table in the Database concept View .

-----------------   
What are documents?
- Documents Are the things that we are searching for .
- They can be more than text - any structured JSON data works .
- Every Document has a unique Id and a type .
----------------- 


What are the Indices?

- An Index can power the search for the documents within a collection of the types.
- The inverted indices that are created allows to search across them everything at a single go .


What is The Term Frequecy? Document Frequency and What is the concept of the relavance ?


Term Frequency: 
   - This usually indicates the number of times that a term would appear in the document 

Document Frequency?
   - The number of times that a term can appear in the overall documents 


Relavance?
  - Term Frequency/DocumentFrequency 


What purpose do the Inverted Indices Serve?
  - The inverted indices basically map the given search terms to the Documents .

An Index Configured for 5 primary and 3 replicas would have how many shards in total?
  - So As there are 3 replica shards should be there for every primary shard so there are 15 replica shards and then 5 primary shards whose cummulative sum would be 20 shards in total .


Is elastic Search only used for full text search?
  - No the elastic search is used not only for full text searching but also used for the sake of the documents and their searching and the aggregating the functions and for sending the data that is useful for the analytics .

What is A Mapping In ElasticSearch Server?
   - A mapping is the defining of the schema, Elastic search has its defaults but sometimes we need to customize them .

```
   CURL -XPUT http://127.0.0.1:9200/movies -d '
   {
      "mappings": {
          "properties": {
              "year": {
                  "type": "date"
              }
           }
      } 
   } 
```

What is CommonMappings ?
  - Here are some of the Common Mappings that we can find like 
  FieldTypes:
    - string, byte, long, float,double, boolean, date
    ```
      "properties": {
          "created_at": {
             "type": "date"
          }
      }
    ```
  FieldIndex:
    - Do you want to index a field for a full text search? analyzed/not analyzed/no 
    ```
      "properties": {
          "created_at": {
              "index"
          }
      }
    ```



More About Analyzers?
  - Analyzers are pretty much helpful in analyzing the data . There are various kinds of the Analyzers .
    - Character Filters:
        * Removes HTML encoding, converts & to and
    - Tokenizer:
        * Splits strings based on the white spaces/punctuation/non-letter characters so that it would be very easy to analyze the data .
    - Token Filter:
        * This can be done with the techniques of stemming, Lowercasing, stop words, synonyms.
        Stemming is nothing but the process of finding the common term that we search lets say for example there is a term for search as "box"
        and the words like boxing and boxes and the boxer would also be a map of the analyze and select for the taking and this is taken care by stemming .

        Lowercasing when we want the search to tbe canse insensitive then we go for this .
    
        stopWords: like that do not really convey much like "to", "the", "an" can be made exclusive out for searching by this .

        synonms: Can be specified and the matching words for those would also acts as a thing for the reference .


What are the Choices for the Analyzers?
  - Standard
     This Type of the Analyzer does the process of the removing punctuation  
  - Simple

  - White Space
  - Language


For the First Time in MyLife i have created a schema in elasticsearch for a field
```
 curl -XPUT http://127.0.0.1:9200/movies -H 'Content-Type: application/json' -d '
    {
       "mappings":
            {"properties":
                    { "year":{
                         "type": "date"}
                    }
            }
}'
```

whose output is 
```
{"acknowledged":true,"shards_acknowledged":true,"index":"movies"}

```


Posting of the data that mean a document into the field mapping for the movies would be  something of this sort .

```
  curl -XPOST http://localhost:5200/movies/_doc/10324 -d '
  {
     "genre": ["IMAX", "SCI-FI"],
     "title": "Interstellar",
     "date": 2020 
  }
  '
```

GEtting of the Movie document from the index 
```
  curl -XGET 'http://localhost:9200/movies/10324' 

```

----------------------------------------------------
CREaTING ONE doc OF MOVIE
```
curl -XPOST http://localhost:9200/movies/_doc/1025 -H 'Content-Type: application/json' -d '
{
  "genre": ["IMAX", "Fiction"],
  "date": 2020,
  "title": "Sherlock"
}
'

```
The output response for the above insertion is 
```
 {"_index":"movies","_type":"_doc","_id":"10245","_version":1,"result":"created","_shards":{"total":2,"successful":2,"failed":0},"_seq_no":3,"_primary_term":2}
```



# Update Operation in the Movies In Elastic Search:
   In elastic search we cannot update an already existing records instead the updating here would be done by updating the version which means that the entire document would get created but with the different version and the old document is marked for the sake of the deletion.
```
  # it should XPOST when doing the partial update
  curl -XPOST 'http://localhost:9202/movies/_doc/10497/_update' -d '
  {
    "title" : "Interstellar FFFFFFFF"
  }
  '

  # when the record is fully updated then we use 
  curl -XPUT 'http://localhost:9202/movies/_doc/10497?pretty' -d '{
    "title": "Interstellar Fully Update"
    "genre": ["Sci-Fi", "Fiction"]
    "year": 2014
  }
  '
```


Deleting of the Record in the Elastic SEarch
```
  $ curl -XDELTE 'http://localhost:9202/movies/'
```




When record is being Deleted then we use 

