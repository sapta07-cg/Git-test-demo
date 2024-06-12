# Git-test-demo

This is my first Git repo


https://www.youtube.com/watch?v=XQmM0oF8dhs

https://www.youtube.com/watch?v=2JBx_06dD1k

https://www.youtube.com/watch?v=8vNFuUALYv4

https://www.youtube.com/watch?v=OJ1tkK3mJSw&t=2004s

https://www.youtube.com/results?search_query=crud+operation+with+api+using+rtk+query+piyush+garg


https://www.youtube.com/watch?v=YQD3AgzjwUg


Node Tutorial 2.........................




1) Connect database with express
   -----------------------------
   We connected our application with database using Mongoose. 

   Mongoose is an Object data modeling(ODM) library for MongoDB and Node js, a higher level of abstraction over regular mongo db driver. 

   Mongoose allows for rapid and simple development of MongoDB database interaction. 

   Features: schemas to model data and relationships, easy data validation, simple query API, middleware etc.

   Mongoose schema:
   ---------------
   Here we model our data by describing the structure of data, default values and validation. 

   Mongoose model:
   --------------
   a wrapper for the schema, providing an interface to the database for CRUD operations. It's like a blueprint that we use to create document.   

   Ex:
   --
   const { json } = require("express");
   const express = require("express");
   const fs = require("fs");
   const app = express();
   const mongoose = require("mongoose");

   const users = require("./MOCK_DATA.json");

  //Schema

  const userSchema = new mongoose.Schema(
  {
    firstName: {
      type: String,
      required: true,
      //requied:[true,'firstName must be required'] --> We can also set error msg
      // default: "None"
    },
    lastName: {
      type: String,
    },
    email: {
      type: String,
      required: true,
      unique: true,
    },
    gender: {
      type: String,
      required: true,
    },
    jobTitle: {
      type: String,
    },
  },
  { timestamps: true }
);

//Model

const User = mongoose.model("user", userSchema);

//Conection

mongoose
  .connect("mongodb://localhost:27017/cgemployee")
  .then(() => console.log("MongoDB connected"))
  .catch((err) => console.log("Mongo err", err));

//MIDDLEWARE-plugin
app.use(express.urlencoded({ extended: false }));
app.use(express.json()); // this middleware used to send data in post method as json
app.use((req, res, next) => {
  console.log("Hello from M1");
  req.myUserName = "Sapta";
  next(); // it calls the next middleware, if middleware not there then it calls the route function
});

app.use((req, res, next) => {
  console.log("Hello from M2", req.myUserName); //Hello from M2 Sapta
  next(); // it calls the next middleware, if middleware not there then it calls the route function
});

const checkBody = (req, res, next) => {
  if (!req.body.email || !req.body.gender) {
    return res.status(400).json({
      status: "fail",
      message: "Missing mail or gender",
    });
  }
  next();
};

const userRouter = express.Router();

// functions

const createUser = async (req, res) => {
  const body = req.body;
  const result = await User.create({
    firstName: body.firstName,
    lastName: body.lastName,
    email: body.email,
    gender: body.gender,
    jobTitle: body.jobTitle,
  });

  console.log("User detail", result);
  return res.status(201).json({
    msg: "success",
  });
};

const getAllUsers = async (req, res) => {
  const allUsers = await User.find({});
  return res.status(200).json({
    status: "success",
    results: allUsers.length,
    data: {
      allUsers,
    },
  });
};

const getSingleUser = async (req, res) => {
  // const id = req.params.id * 1; // we have use numbers because id is coming as string (We can also like that: req.params.id * 1 )

  // const user = users.find((item) => item.id === id);

  const user = await User.findById(req.params.id);

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
};

const deleteUser = async (req, res) => {
  // const id = req.params.id * 1;
  // const user = users.filter((item) => item.id != id);
  // fs.writeFile("./MOCK_DATA.json", JSON.stringify(user), (err, data) => {
  //   return res.status(200).json({
  //     status: "success",
  //     message: `Deleted record ${id}`,
  //   });
  // });

  await User.findByIdAndDelete(req.params.id);
  return res.status(200).json({
    status: "success",
    message: `Deleted record ${req.params.id}`,
  });
};

// Route defines

userRouter.route("/").get(getAllUsers).post(checkBody, createUser);

userRouter.route("/:id").get(getSingleUser).delete(deleteUser);

app.use("/users", userRouter);

app.listen(8000, () => {
  console.log("Server started");
});


2) Model View Controller (MVC pattern)
   -----------------------------------
   Model View Controller consists of three components, Model(Business logic), View(Presentation logic) and controllers(Application logic). Controllers manipulates the model and model updates the view. One of the big goal of MVC model to separate the application and business logic

   Using MVC pattern we can refactor our code. For that we need to create some folders and those are controllers, ,models, views, routes etc. 

 It starts with a request. Request hits one of our router. Now the goal of the router is to delegate the request to the correct handler function which will be in one of the controllers. There will be one controller for each of our resources to keep diff parts of the app nicely separated. Then depending on the incoming req, controller might need to interact with one of the models. After getting the data from model, the controller might then be ready to send back a response to the client. 

After getting data from model, the controller then select one of the view template and inject data into it. 

Application logic & Business logic
----------------------------------
Application logic
-----------------
i) Code that is only concerned about the application's implementation, not the underlying business problem we are trying to solve

ii) Concerned about managing req and res 

iii) About the app's more technical aspect. 

iv) bridge between model and view layer.

Businesslogic
-------------
i) Code that is only concerned about the business problem, how the business works and business needs

   Eg: creating new collection in database, check user's password is correct, validations logic etc.

Fat models/thin controllers : offload as much as possible into the models and keep the controllers as simple and lean as possible.





   














