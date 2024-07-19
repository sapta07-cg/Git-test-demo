# Git-test-demo

This is my first Git repo


https://www.youtube.com/watch?v=XQmM0oF8dhs

https://www.youtube.com/watch?v=2JBx_06dD1k

https://www.youtube.com/watch?v=8vNFuUALYv4

https://www.youtube.com/watch?v=OJ1tkK3mJSw&t=2004s

https://www.youtube.com/results?search_query=crud+operation+with+api+using+rtk+query+piyush+garg


https://www.youtube.com/watch?v=YQD3AgzjwUg


19) Aggregation
    -----------
   Aggregation is the process of selecting data from a collection to process multiple documents and returns computed results. MongoDB process the data records in the database during an aggregation operation and returns a single computed results. 

Aggregation operation allow us to group, sort, perform calculations, analyze data and much more. 

why we use Aggregation
----------------------
i) Group values from multiple documents together.

ii) Fetching a lot of nested data to perform complex operations. 

iii) Filter and sort documents and analyze data changes. 

Aggregation pipeline
--------------------
An aggregation pipeline consists of one or more stages that process documents:

Each stage performs an operation on the input documents. For example, a stage can filter documents, group documents, and calculate values.

The documents that are output from a stage are passed to the next stage.

An aggregation pipeline can return results for groups of documents. For example, return the total, average, maximum, and minimum values.

** the order of these stages are important. Each stage acts upon the results of the previous stage. Basically Aggregation pipeline is nothing but a collection of stages. Aggregation applies on collection, not database. 

  MongoDB Collection --> Stage1 --> Stage2 --> ....-> Stage n --> Result

     (Stage1 --> Stage2 --> ....-> Stage n) --> Aggregation pipeline

Ex:  input --> $match --> $group --> $sort --> $project --> output

Command: db.collectionName.aggregate(pipeline,options)

  pipeline = [
  {
    $match: {
      ...
    }
  },
  {
    $group: {
      ... 
    }
  },
  {
    $sort: {
      ... 
    }
  }
]

Aggregation works in Memory. Each stage can use up to 100 MB of RAM, we will get an error from database if we exceed the limit. 

Ex:  [
  {
    $match: {
      gender: "Female"
    }
  },
  {
    $sort: {
      salary: 1
    }
  }
]
  
why we use aggregation instead of find query?
--------------------------------------------
The main purpose of the aggregation framework is to ease the query of a big number of entries and generate a low number of results. For some times find() method is not sufficient for query especially for complex data. In that case we can use aggregation.  








   














