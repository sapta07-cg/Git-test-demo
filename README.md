# Git-test-demo

This is my first Git repo


Express
-------
Express is a minimal node.js framework which means it is actually built on top of node js. Bascially it's a higher level of abstraction. 

Express contains a very robust set of features: complex routing, easier handling of requests and responses, middleware, server side rendering etc. 

Express makes it easier to organize our application into the MVC architecture. 


Route definition takes the following structure:

 app.METHOD(PATH, HANDLER)

Where:

app is an instance of express.

METHOD is an HTTP request method, in lowercase.

PATH is a path on the server.

HANDLER is the function executed when the route is matched. 

Ex:
--
const express=require('express');  // require express package
const app = express();

app.get('/',(req,res)=>{
  res.status(200).json({message: 'This is home page',app: 'test'})

})

app.get('/about',(req,res)=>{
  res.send(`Hello this is about page ${req.query.name}`);

})


app.listen(8000,()=>{console.log("Server started")});


2) Rest API
   --------
   API:
   ---
   API stands for Application Programming Interface. It's bascially a piece of software that can be used by another piece of software, in order to allow applications to talk each other.

API always aren't related to send data and used in web applications. The others API are Browser's DOM JS API, With OOPs programming when exposing methods to pubic we are creating API etc.

REST api stands for Representational State Transfer, is bascially a way of building web api in a logical way making them easy to consume. 

REST Architecture
-----------------
i) We need to separate our API into logical resources. 

   The key abstraction of information in REST is a resource and therefore all the data that we want to share in the API should be divided in to logical resources. Resource is a object or representation of something, which has some data associated to it.  

ii) The resources should be exposed using structured, resource-based URLs.

iii) To perform different action of data, API should use right http methods. 

iv) send data as json(usually)

    JSON is a very lightweight data interchange format which is heavily used by web APIs. In JSON, all the keys have to be string. 

v) API must be stateless.

   A REST API's all states is handled on the client and not on the server. This means that each request must contain all the info necessary to process a certain request. The server should not have to remember previous requests.  State refers to piece of data that might change over time. 

** REST API works on Server Client Architecture. It means Server and Client are different entity and it should not be dependent on each other. Suppose we send data in html then it won't be worked in any mobile application. So in REST API, this shouldn't be happened. Mostly we send data as JSON format. 

** We should always respect all http methods.Suppose we want to update data, then we should use PATCH menthod and shouldn't use other method. 

** Postman is a utility tool which helps in API testing and documentation. 


Ex of REST API
--------------
const { json } = require("express");
const express = require("express");
const fs = require("fs");
const app = express();

const users = require("./MOCK_DATA.json");

//MIDDLEWARE-plugin
app.use(express.urlencoded({ extended: false }));
app.use(express.json()); // this middleware used to send data in post method as json

// List of all users

app.get("/users", (req, res) => {
  return res.status(200).json({
    status: "success",
    results: users.length,
    data: {
      users,
    },
  });
});

// Get a single user

app.get("/users/:id", (req, res) => {
  const id = req.params.id * 1; // we have use numbers because id is coming as string (We can also like that: req.params.id * 1 )

  const user = users.find((item) => item.id === id);
  if (!user) {
    return res.status(404).json({
      status: "fail",
      message: "ID not found",
    });
  }
  return res.status(200).json({
    status: "success",
    data: {
      user,
    },
  });
});

//Add a user

app.post("/users", (req, res) => {
  const body = req.body;
  console.log(body);
  users.push({ ...body, id: users.length + 1 });
  fs.writeFile("./MOCK_DATA.json", JSON.stringify(users), (err, data) => {
    return res.status(201).json({
      status: "success",
      id: users.length,
      data: {
        body,
      },
    });
  });
});

// update a user

//delete a user

app.delete("/users/:id", (req, res) => {
  const id = req.params.id * 1;
  const user = users.filter((item) => item.id != id);
  fs.writeFile("./MOCK_DATA.json", JSON.stringify(user), (err, data) => {
    return res.status(200).json({
      status: "success",
      message: `Deleted record ${id}`,
    });
  });
});

app.listen(8000, () => {
  console.log("Server started");
});


** We can also use below process

app.route('/users').get(...rest code).post(...rest code)


3) Middleware
   ----------
   Whenever a request comes, then at first it passes through the middlewares and the middlewares process on the request. If the request is validated suceesfully by middleware, then its send the request to function and otherwise its return the request to client and end the req-res cycle.

** A middleware is a function that runs for every request. It's like a middle man between req and res.It's executed in middle of receiving the request and sending the response. We can use multiple middleware in same code. All the middlewares used in our app is called "Middleware stack".

**** In express, Everything is middleware(even routes).

   Req ---> M1 --> M2 --> M3 ..  --> Response
    

Middleware functions are functions that have access to the request object (req), the response object (res), and the next function in the application’s request-response cycle. The next function is a function in the Express router which, when invoked, executes the middleware succeeding the current middleware.

Middleware functions can perform the following tasks:

 i)Execute any code.

ii) Make changes to the request and the response objects.

iii) End the request-response cycle.

iv) Call the next middleware in the stack.

** If the current middleware function does not end the request-response cycle, it must call next() to pass control to the next middleware function. Otherwise, the request will be left hanging. 

**In middlewares , Order is IMPORTANT.

** use() method is used to add middleware in our middleware stack.

** we can also use 3rd party middleware. 

Ex:
--
//middleware
app.use((req, res, next) => {
  console.log("Hello from M1");
  req.myUserName = "Sapta";
  next(); // it calls the next middleware, if middleware not there then it calls the route function
});



return res.status(200).json({
    status: "success",
    passedData: req.myUserName,
    data: {
      user,
    },
  });
   

Creating and mounting routes
----------------------------
We can create one router for each resources. 


const userRouter= express.Router();

userRouter.route('/').get("Function details").post("Function details")

userRouter.route('/:id').patch("Function details").delete("Function details")


app.use('/users',userRouter)

** We can not use the routers before we actually declare them. 

When  this "/users/:id" APi invoked, then the request will enter the middleware stack and when its hits this middleware here, it will run the "userRouter" because this route is matched here. Then its enter the userRouter and according to method and url, it runs the function

param middleware
----------------
param middleware is a middleware that only runs for certain parameters. Bascially when we have a parameter in URL, then we can access that parameter using this middleware.

app.use('id',(req,res,next,val)=>{    //"id" is the parameter name that we actually search for.
  console.log(`User id is ${val}`);
  next();
})  

chaining middlewares:
--------------------
We can add multiple middleware functions in a handler.

Ex:
--
const checkBody = (req, res, next) => {
  if (!req.body.email || !req.body.gender) {
    return res.status(400).json({
      status: "fail",
      message: "Missing mail or gender",
    });
  }
  next();
};

userRouter.route("/").get(getAllUsers).post(checkBody, createUser);

here checkBody middleware wil run before a user created. 

** if we want to use static file (Ex: HTML file), then we have to use below middleware:

     app.use(express.static("file location"));

HTTP headers
------------
HTTP headers are an important part of the API request and responses as they represent the meta-data(data about data) associated with the API request and response. 

Headers carry info about for the request and response body. 

"user-agent" in req heares is define that which application is used to make the request.

We can also set custom header manually. Ex:

res.setHeader("X-myName":"Sapta"); // Always add X to custom header.

HTTP status code
----------------
HTTP response status codes indicate whether a specific HTTP request has been successfully completed. Responses are grouped in five classes:

Informational responses (100 – 199)

Successful responses (200 – 299)

Redirection messages (300 – 399)

Client error responses (400 – 499)

Server error responses (500 – 599)

201 -- it's used when we create something. 

400-- Bad request

401-- Unauthorized

402-- Payment required

500-- Internal server error



Environment variables:
---------------------
Environment variables are global variables that are used to define the environment in which a node app is running. 




