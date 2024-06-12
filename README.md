# Git-test-demo

This is my first Git repo


https://www.youtube.com/watch?v=XQmM0oF8dhs

https://www.youtube.com/watch?v=2JBx_06dD1k

https://www.youtube.com/watch?v=8vNFuUALYv4

https://www.youtube.com/watch?v=OJ1tkK3mJSw&t=2004s

https://www.youtube.com/results?search_query=crud+operation+with+api+using+rtk+query+piyush+garg


https://www.youtube.com/watch?v=YQD3AgzjwUg


MongoDB Tutorial.............................



1) MongoDB means Humongous, which means extremely large. It's a most useful and trending the database.

2) The vendor of MongoDB is MongoDB.

3) We can use MongoDB for desktop application, mobile application etc. and for web application this database is more popular.

4) Full stack application, stack means the technologies which can be used to develop web application are called a stack. 

   The most popular stack:
   
   MEAN, MERN

5) MongoDB also based on JavaScript. It internally used Mozilla's Spider monkey Javascript engine. 

6) Type of MongoDB: Document Database/ NoSql Database

7) Relational Database vs Document Daabase
   ---------------------------------------
   There are 2 most common types of database.

   1.Relational Database
   
   2.Document Database/ NoSql Database

   Relational Database:
   -------------------

   Here the data will be stored in tables and the tables have fixed schema(scheam means structure). 

   Ex: Employee(eno, name, salary, address)

   The data stored in tables has relationship like:

    One to One, One to Many, Many to one etc. 

   To retrieve data from relational database, we have to write join queries which collects data from different tables. 

   Eg: Oracle, MySQL etc.



   Document Database:
   -----------------
   
   Here each database contains multiple collections. Inside each collection, there are multiple documents. 

   Collection : Table in realtional database

   Document: Row in table 

   Data will be stored in separate documents and each document is independent of others. Document database don't have fixed schema. 

   Not required to join queries --> performance is more

   Eg: MongoDB, Cassandra etc  

   MongoDB structure:
   -----------------

       
         MongoDB --> Database 1,Database 2,.... --> Collection 1, Collection 2,... --> Document 1,  Document 2,....

  MongoDB physical database contains several logical databases. 

  Each database contains several collections. Collection is something like table in relational database. 

  Each collection contains several documents. Document is something like row/record in relational database. 

  Eg:
  
  Database: Shopping cart database
  Collections: Customres, Products, Orders
  Customres : several documents

  document1:{
    
    Name: "Sapta",
    age: 26,
    Company: CG
  }

  document1:{
    
    Name: "Sapta",   // there is no fixed schema
  }

  How data represent in MongoDB
  -----------------------------
  In JSON(BSON) format

  JSON --> Javascript Object Notation (json is understandable by any programming language)
  
  BSON --> Binary JSON

8) Key characteristics of MongoDB
   ------------------------------
   i) Installation and setup is very easy.

   ii) All information related to a document will be stored in a single place. To retrieve data, it is not required to perform join operation and retrieval is very fast. (It's a biggest advantage)

  iii) Documents are independent of each other and no schema. Hence we can store unstructured data like videos, audio files etc.

  iv) We can perform operations like editing existing documents, deleting document and inserting new document very easily. 

  v) Retrieval data is in the form of JSON which can be understandable by any programming language without any conversion. (Interoperability)

  vi) We can store very huge amount of data and hence scalability is more.


**** Note : Performance and Flexibility are biggest assets of MongoDB. 

9) There will be only one physical database beacuse only one installation of MongoDB. But there will be more than one logical databases(eg: stode db, college db etc.). 

In the logical database, we can create any number of collections(tables).

 MongoDB(Physical database)  --> Logical databases --> Collections --> Documents

 Collection --> Tables

 Documents --> Record/Row

 NoSQl --> Not only SQL

10) MongoDB Shell & MongoDB Server
    ------------------------------
    Once we installed MongoDB, We will get MongoDB Shell and MongoDB server. These are javascript based applications.

    MongoDB server is responsible to store our data in database. 

    MongoDB shell is responsible to manage server. Using shell, we can perform all required crud operations.

    ** In MongoDB, all crud operations will be realted to documents. 

    ** MongoDB server can be local or remote. 


11) To launch/start MongoDB server --> mongod command

    To launch/start MongoDB shell --> mongo command


    GUI support is also there for mongoDB shell --> Compass, Robo T3 etc. 

   ** If we want to communicate with MongoDB from any kind of applications then we need a special software called Driver. Driver is responsible to provide data to application from database. 

      Application(Java/python/JS)  ---> Driver --> MongoDB

  ** 27017 is the default port number for mongodb.


12) MongoDB uses a data format similar to JSON for data storage called BSON (Binary JSON). It's look bascially same as JSON but it's typed, meaning that all values will have a data type such as string, Boolean, date, integer or more. 

 ** In BSON, the maximum size for each document is currently 16 mb but this might be increase in future.

 ** Each document contains a unique id which acts as a primary key of document. It's automatically generated with the Object id datatype each time there is a new document. Object id is BSON type, it's not JSON type
  
 Object is of 12 bytes.

 i) The first 4 bytes represents the timestamp when this document was inserted.

 ii) The next 3 bytes represents machine identifier(host name)

 iii) The next 2 bytes represents process id.

 iv) The last 3 bytes represents some random increment value.

  db.collection.find()[0]._id.getTimeStamp() ---> it provides timestamp of document

 ** Object id is unque as per collection not database.

 Q) Why this lengthy object
    -----------------------
   The only reason is uniqueness. 

 ** In JS, only 6 types are available: string, number, object, array, Boolean,null

    But BSON provides some extra types:
    
    32-bit-integer --> NumberInt , ObjectId, Date etc.

** BSON format requires less memory

   JSON --> 10KB

   BSON --> 4-5 KB
 
**** Efficient storage and extra datatypes are speciality of BSON over JSON.

Q) while retrieving also BSON will be converted to JSON by MongoDB server?

Ans:   At the time of retrieval, BSON data will be converted to EJSON for understanding purpose.  

     EJSON --> Extended JSON

 Insertion of document/Creation --> JSON to BSON

 Read operation/ Retrieval operation --> BSON to EJSON

 ** There are 3 types of data formats in MongoDB.

    JSON, BSON, EJSON

  



    


13) Command to check the databases:

    show dbs

    Output:

    admin
    config
    local

    Mongo Admin will use these default databases.

   admin
   -----
   admin database is used to store user authentication and authorization info like username, password, roles etc.

   This database is used by administrators while creating, deleting and updating users and while assigning the roles. 

   config
   ------
   to store configuration info of mongodb server. This database is used by admin only.

   local
   -----
   local database can be used by admin while performing replication(create a duplicate copy) process.

   
14) Creation of database and collection:
    -----------------------------------
   Database won't be created at the beginning and it will be created dynamically. Whenever we are creating collection or insert document then database will be created dynamically. 

Create or switch to database:

db."databasename"  (db.capgeminidb)  --> this will not show at beginning. whenever we create collection, then only it shows in list of databases.

Create collection:

db.createCollection("employees");

Show databases:

show dbs

Output:

    admin
    config
    local
    capgeminidb

Show collections:

show collections

Output:

  employees

** "db" is an implicit object in MongoDB. That's why it's used to create database. 

Drop collection:

 db.collection.drop();  --> db.employees.drop();

Drop.database:

 db.dropDatabase(); --> 

current database will be deleted

** db.getName()   --> provide the current name of database


15) CRUD operation
    -------------- 

   i) Create || insert document
      -------------------------
   How to insert document into the collection

   db.collection.insertOne()
   db.collection.insertMany([]); --> here document should be an array
   db.collection.insert();  --> to insert either a single document or multiple documents

   db.employees.insertOne({eno: 100, ename:"Sapta", esal:1000})

   db.employees.insertOne([{},{},{},{}])

   db.employees.insert([{},{},{},{}])
   db.employees.insert({})

   Ex:
   --
   var emp={
    ename:"Sapta",
    esal:1000
   }

   db.employees.insertMany([emp])
   db.employees.insertOne(emp)

  ** if we want to insert a json file into database, then we have to use below command:

     load("file_path");
    
    In JSON file, the data should be in array form and the data should be json only.

   ii) Read || Retrieval operation
       ---------------------------
    db.collection.find() --> to get all documents present in the given collection

    db.collection.findOne() --> to get only one document

    eg: db.employees.find()

        db.employees.find().pretty()

      db.employees.find({ename:"Sapta"})

   iii) update operation
        ----------------
      db.collection.updateOne()
      db.collection.updateMany();
      db.collection.replaceOne()

     db.employees.updateOne({ename:"Sapta"},{$set:{esal:10000}})

     if esal field is available then old value will be replaced with 10000. Otherwise a new field will be created. 

     ** If anything prefix with $ symbol, then it is predefined word in MongoDB.

     ** Object Id can not be updated.

  iv) Delete operation
      ----------------
    db.collection.deleteOne()
    db.collection.deleteMany();
     
     db.employees.deleteOne({ename:"Sapta"})

16) Capped collection
    -----------------
    We can provide some option during create the collection and this collections are called capped collection.

    db.createCollection(name,option)

    db.createCollection("employees",{capped:true, size: 3736578, max: 1000});

  For capped collection, if it's exceeding the size and cross the number then old document(based on timestamp) will be deleted automatically. "capped" should be true.

** If capped is true then "size" field is required.     

   db.createCollection("employees",{capped:true}); --> this is invalid

   db.createCollection("employees",{ size: 3736578, max: 1000});--> this is invalid. Without capped value we can't use size or max

** If size exceeds or maximum number of documents reached, then oldest entry will be deleted automatically, such types of collections are called capped collection. 

17) JSON va JS object
    -----------------
    In JS object, quote symbol for keys are optional. But in JSON, quote symbol for keys is mandatory

    db.collection.insertOne()

    mongoimport --> tool to import documents from json file into MongoDB.

    Ex:
   
    storedb --> database name
    books --> collection

    mongoimport --db storedb --collection books --file book.json --jsonArray

18) Nested documents
    ----------------
   Sometimes we can take a document inside another document, such type of documents are called nested document or embedded document. 

   employee:{
   
    ename:"Sapta"
    esal:10000
    hobbies:{h1:"playing",h2:"reading"}

   }

   MongoDB supports upto 100 level of nesting.

   A document can contain array also. 









