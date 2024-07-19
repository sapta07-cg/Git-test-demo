# Git-test-demo

This is my first Git repo


https://www.youtube.com/watch?v=XQmM0oF8dhs

https://www.youtube.com/watch?v=2JBx_06dD1k

https://www.youtube.com/watch?v=8vNFuUALYv4

https://www.youtube.com/watch?v=OJ1tkK3mJSw&t=2004s

https://www.youtube.com/results?search_query=crud+operation+with+api+using+rtk+query+piyush+garg


https://www.youtube.com/watch?v=YQD3AgzjwUg


3) Filtering in API
   ----------------
   userController.js

   const queryObj = { ...req.query }; // used for query obj

   const query = User.find(queryObj);

  Advance filtering
  -----------------
  find(gender: Female, {salary:{$gte:20000}}) --> find a document where salary greater than or equal to 20000. 

  const queryObj = { ...req.query }; // used for query obj
  console.log(req.query);

  let queryStr = JSON.stringify(queryObj);
  queryStr = queryStr.replace(/\b(gte|gt|lt|lte)\b/g, (match) => {
    `$${match}`;
  });
  let newQueryObj = JSON.parse(queryStr);

  console.log("new obj", newQueryObj);

  const allUsers = await User.find(newQueryObj);


  We can also use special mongoose function.

  const queryObj = { ...req.query }; // used for query obj
  console.log(queryObj);

  const allUsers = await User.find()
    .where("gender")
    .equals(queryObj.gender)
    .where("salary")
    .gte(queryObj.salary);

URL: http://localhost:8000/users?gender=Female&salary[gt]=20000

4) Sort API
   --------
   http://localhost:8000/users?sort=gender

   const getAllUsers = async (req, res) => {
   console.log(req.query.sort);

   let query = User.find();

   query = query.sort(req.query.sort);

   const allUsers = await query;

   return res.status(200).json({
     status: "success",
     results: allUsers.length,
     data: {
       allUsers,
     },
   });
  };

  for multiple sort:

  When we want sort by two or more fields, we need to specify those fields separated by space to the sort methods.

  query.sort('firstName age')

  Ex:

  const getAllUsers = async (req, res) => {
  let query = User.find();

  const sortBy = req.query.sort.split(",").join(" "); 

  query = query.sort(sortBy);

  const allUsers = await query;

  return res.status(200).json({
    status: "success",
    results: allUsers.length,
    data: {
      allUsers,
    },
  });
};

if anything is not mentioned then sort against created date:

  let query = User.find();

  query = query.sort("-createdAt");

  const allUsers = await query;

5) Limiting fields
   ---------------
   Using this, we can only return the limited fields which are mentioned in URL.

   Ex:
   --
   const getAllUsers = async (req, res) => {
   let query = User.find();

   const fieldsby = req.query.fields.split(",").join(" ");

   query = query.select(fieldsby); --> we are using select query mehtod to return the value.

   const allUsers = await query;

   return res.status(200).json({
    status: "success",
    results: allUsers.length,
    data: {
      allUsers,
    },
  });
};

URL: http://localhost:8000/users?fields=firstName,lastName

** We have to only mention either inclusion fields or exclusion fields, we can not have mixture of both. If we try like below, then it will not work.

  http://localhost:8000/users?fields=-firstName,lastName  --> this will not work

** We can also exclude fields from schema. Like password we should not show to client. To do that, we have to do below--

  firstName:{
   select: false
  }

6) Pagination
   ----------
  Here we are using two functions, skip() and limit()

  skip() --> here we need to mention how many record we have to skip

  limit() --> here we have to specify how many records we want in result.

  Ex:

  const getAllUsers = async (req, res) => {
  let query = User.find();

  //Pagination
  const page = req.query.page * 1 || 1;

  const limit = req.query.limit * 1 || 10;

  //page1: 1-10 2: 11-20 3: 21-30 4: 31-40

  const skip = (page - 1) * limit;  --> this is the logic, for 2nd page: (3-1)*10=20, here first 20 records will skip 

  query = query.skip(skip).limit(limit);

  const allUsers = await query;

  return res.status(200).json({
    status: "success",
    results: allUsers.length,
    data: {
      allUsers,
    },
  });
};

if there is no record in page, then we can use below logic.

 let query = User.find();

  //Pagination
  const page = req.query.page * 1 || 1;

  const limit = req.query.limit * 1 || 10;

  //page1: 1-10 2: 11-20 3: 21-30 4: 31-40

  const skip = (page - 1) * limit;

  query = query.skip(skip).limit(limit);

  if (req.query.page) {
    const userCount = await User.countDocuments();
    if (skip >= userCount) {
      console.log("This page is not found"); --> here we can throw error and return the error response
    }
  }

  const allUsers = await query;

 URL: http://localhost:8000/users?page=2&limit=2

7) Aliasing a route
   ----------------
  There is a situation like a specific route might get lot of requests and for that route might be complex with lot of query string , we can provide a simple alias to easily access it. 

here we creating a alias of top employees:

userRouter.js

router.route("/topemployees").get(getTopEmployees, getAllUsers);

userController.js

const getTopEmployees = (req, res, next) => {
  req.query.limit = 3;

  next();
};


const getAllUsers = async (req, res) => {
  let query = User.find();

  console.log("Value from middleware", req.query.limit);

  //Pagination
  const page = req.query.page * 1 || 1;

  const limit = req.query.limit * 1 || 10;

  //page1: 1-10 2: 11-20 3: 21-30 4: 31-40

  const skip = (page - 1) * limit;

  query = query.skip(skip).limit(limit);

  const allUsers = await query;

  return res.status(200).json({
    status: "success",
    results: allUsers.length,
    data: {
      allUsers,
    },
  });
};

URL: http://localhost:8000/users/topemployees

Output: we will get 3 emps details


** here we are writing all these features logic in same function which is not right approach. So we will create a class and in that we will add all featues, so that we can use further in another model. 

8) Aggregation pipeline
  --------------------
 check aggregation pipeline in MongoDB note.

  $match and $group
  -----------------
  Mongoose gives us access to aggregation pipeline through aggregate() function. 

  Ex:
  --
  const getUserStats = async (req, res) => {
  const stats = await User.aggregate([
    {
      $match: {
        salary: { $gt: 20000 },
      },
    },
    {
      $group: {
        _id: "$jobTitle",   // here group all the data based on "id" value
        avgSalary: {
          $avg: "$salary",
        },
        maxSalary: {
          $max: "$salary",
        },
        minSalary: {
          $min: "$salary",
        },
        empCount: {
          $sum: 1,
        },
      },
    },
    {
      $sort: {
        minSalary: 1,
      },
    },
    {
      $match:{...}
    }
  ]);
  return res.json({
    status: "success",
    data: {
      stats,
    },
  });
};

  at first it's filtered with $match value and on return result, group operation will work. 

  We can also repeat the stages.

 $unwind and $project
 --------------------
 $unwind
 -------
unwind stage basically destructure a documents based on the fields which is assigned with an array and it creates multiple documents from a single documents based on that array field.

When the operand does not resolve to an array, but is not missing, null, or an empty array, $unwind treats the operand as a single element array.


Ex:
--
db.inventory.insertOne({ "_id" : 1, "item" : "ABC1", sizes: [ "S", "M", "L"] })


db.inventory.aggregate( [ { $unwind : "$sizes" } ] )

result:

{ "_id" : 1, "item" : "ABC1", "sizes" : "S" }
{ "_id" : 1, "item" : "ABC1", "sizes" : "M" }
{ "_id" : 1, "item" : "ABC1", "sizes" : "L" }

Missing or Non array values:

db.clothing.insertMany([
  { "_id" : 1, "item" : "Shirt", "sizes": [ "S", "M", "L"] },
  { "_id" : 2, "item" : "Shorts", "sizes" : [ ] },
  { "_id" : 3, "item" : "Hat", "sizes": "M" },
  { "_id" : 4, "item" : "Gloves" },
  { "_id" : 5, "item" : "Scarf", "sizes" : null }
])

{ _id: 1, item: 'Shirt', sizes: 'S' },
{ _id: 1, item: 'Shirt', sizes: 'M' },
{ _id: 1, item: 'Shirt', sizes: 'L' },
{ _id: 3, item: 'Hat', sizes: 'M' }

Documents "_id": 2, "_id": 4, and "_id": 5 do not return anything because the sizes field cannot be reduced to a single element array.


Ex in Node:

const unwindValue=req.params.val

const value=await User.aggregate([{
$unwind:{...}

},{

}

])

$project
--------
project stage is used to tell which fields to be want in results. 

const getUserStats = async (req, res) => {
  const stats = await User.aggregate([
    {
      $match: {
        salary: { $gt: 20000 },
      },
    },
    {
      $project: {
        _id: 0,    // here id field will not show in result as it's set 0. If we put 1, then only _id field will show in result
      },
    },
  ]);
  return res.json({
    status: "success",
    data: {
      stats,
    },
  });
};

limit:

{
    $limit: 1  // set the number of result 
  }


9) Virtual properties
   ------------------
   Virtual properties are document properties that do not persist or get stored in the MongoDB database, they only exist logically and are not written to the document’s collection.

With the get method of virtual property, we can set the value of the virtual property from existing document field values, and it returns the virtual property value. Mongoose calls the get method every time we access the virtual property.

Ex:

userSchema.virtual("dailySalary").get(function () {
  return this.salary / 30;
});

** here in get function, we are using regular function because with "this" property we can access the current assigned object. In arrow function, It's does not get it's own this keyword. Generally if we want to use this keyword, then we have to use regular JS function.

toJSON:{virtuals: true} : It's mean each time the data is outputted as JSON, the virtuals will be true.

** We can not use virtual properties for querying data, because this properties are not technically part of database. 

10) Middlewares in Mongoose
    -----------------------
   Middleware in Mongoose allows you to define functions that execute before or after certain operations on the database. There are two types of middleware hooks: pre and post.

We can use mongoose middleware to execute some logic between two events.

Pre middleware functions are executed before the specified operation (e.g., save, update, remove). You can use pre middleware to perform actions like validation, encryption, or logging before saving, updating, or removing documents from the database.

Post middleware functions are executed after the specified operation. You can use post middleware to perform actions like logging or sending notifications after saving, updating, or removing documents from the database.

Document Middleware
-------------------
Document middleware in Mongoose allows you to create functions that run before or after certain actions on individual documents. These actions could include things like checking data, changing it, or securing it.

//Pseudo code

userSchema.pre('save', function(next) {
  // Execute pre-validation logic for documents here
  next();
});

userSchema.post('save', function(doc, next) {
  // Execute post-validation logic for documents here
  next();
});

** this "save" event will happen whenever we call .save() or .create() from our express app. This save method will not happen for InsertMany or find method.

Ex of pre:
--
userSchema.pre("save", function (next) {
  this.createdBy = "SAPTA";  // "this" here is pointing to the document which is currently being processed.
  next();
});
 

**In this middleware function we have accessed to the document that is being processed. In this case we are accessing to the document which is going to save.

Ex of post:
--
userSchema.post("save", function (doc, next) {                        // "doc" is currently saved document.
  const content = `Document saved and created by ${doc.createdBy}`;
  console.log(content);
  next();
});



** we can add more than one pre or post logic. They will work sequentially. 

Query Middleware
----------------
Query middleware in Mongoose helps intercept and modify queries before or after they are executed. This allows you to add extra conditions, modify the results, or perform other tasks related to the query process.

//Pseudo code

userSchema.pre('find', function(next) {
  // Execute pre-find logic for queries here
  next();
});

userSchema.post('find', function(result, next) {
  // Execute post-find logic for queries here
  next();
});

**Diff b/w Document and Query middleware is that "this" keyword inside this query middleware will point to a query object, not to a document object. 

Ex:
--
userSchema.pre("find", function (next) {
  this.find({ gender: "Male" });
  next();
});

here it will return all the male employees and it will only applicable for find method. 

Aggregation Middleware
----------------------
Aggregate middleware in Mongoose specializes in managing aggregation operations. It allows you to intercept and adjust the results of aggregation queries.

//Pseudo code

userSchema.pre('aggregate', function(next) {
  // Execute pre-processing logic for aggregation here
  next();
});

userSchema.post('aggregate', function(result, next) {
  // Execute post-processing logic for aggregation here
  next();
});

Ex:
--
userSchema.pre("aggregate", function (next) {
  console.log(this.pipeline());  //  this keyword points to currently processing aggregation object. 
  next();
});

11) Data Validators
    ---------------
   Validation is basically checking if the input values are in the right format for each field in our document schema and also that values have actually been entered for all the required fields.  

Built-in validator:
------------------

**unique is not a data validator. 

maxlength:[100,"err message"]

minlength:[value,"err message"]

age: {
      type: Number,
      min: [18],
      max: [55],
    }

**min and max can be used in number and date types.

Custom validators:
-----------------

age: {
      type: Number,
      validate: function (value) {
        return value >= 18 && value <= 55;
      },
    }

or

age: {
      type: Number,
      validate: {
        validator: function (value) {
          return value >= 18 && value <= 55;
        },
        message: "Age should be b/w 18-55",  // we can also add message 
      },
    },

** We can also use library for validation (Ex: Validator.js)


10) Error handling
    --------------
  
    Unhandled routes
    ---------------
   Here below code is for unhandled routes.

   app.all("*", (req, res, next) => {
    res.status(404).json({
      status: "Fail",
      message: `Can't find ${req.originalUrl} on the server!`,
     });
   });

  ** it should always mention in last. Otherwise all will give 404 error.


 Global Error handling middleware
 --------------------------------
There are two types of error happens in our express app. Operation error and programming error.

 Operation error
 ---------------
 Operation error are the problems that we can predict that will happen at some point in future. We need to handle them in advance. 

 i) user trying to access invalid route.

ii) Inputting invalid data

iii) Application failed to connect server.

iv) Request timeout etc.

programming error
-----------------
Programming error are simply bugs that we programmers by mistake introduce in our code.

i) Trying read property of an undefined value. 

ii) using await without async

iii) Accidently used req.query instead of req.body.


** Global error handling middleware will catch and handle all the errors happening in the application. 

** error parameter going to receive the error object, the error which has occurred. 

** express will only call this middleware when the error occurs. 

 ** next(err) --> When we pass any argument in next(), express will automatically know that there was an error. It will skip all the middlewares and directly call global error handling middleware. 

Ex:
--
app.all("*", (req, res, next) => {

  const err = new Error(`Can't find ${req.originalUrl} on the server!`);
  err.status = "fail";
  err.statusCode = 404;

  next(err);
});

//global error handler middleware

app.use((error, req, res, next) => {
  error.statusCode = error.statusCode || 500;
  error.status = error.status || "fail";
  res.status(error.statusCode).json({
    status: error.statusCode,
    output: error.status,
    message: error.message,
  });
});


creating a custom error class
-----------------------------

All the errors which we are going to create using custom error class, that will be operational errors. 

** stackTrace will tell us where the error is actually happened in the code. 



   














