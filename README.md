# Git-test-demo

This is my first Git repo

We have added to local repo here.

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

    
