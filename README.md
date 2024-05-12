# Git-test-demo

This is my first Git repo

We have added to local repo here.

Node note 1............


1) What is Node JS
   ---------------
   Node js is a javascript runtime built on Google's open source V8 Javascript engine. Node js helps to run javscript outside of browser. Node js is a        runtime environment in which a program written in JS can be executed. 

   V8 engine helps to execute the code outside of the browser. Here the code is parsed and executed. 

    Node js is a runtime environment for javascript.
    
Run node file :  node "file_name"

** "npm" stands for Node Package Manager. Whenever we start a project, then we have to write a command "npm init", which creates a template or file that helps to our project. npm is used to install and manage 3rd party open source packages.

   "package.json" is a configuration file which contain all the dependencies and other info. We can create this file manually but there's a chance of error.      So we should create this file using "npm init".

  ** We can install two types of packages, simple dependencies and dev dependencies. Our project or code depends on these dependencies.

   Dependency vs Dev dependency
   ---------------------------
   i)Dependencies are required to run, devDependencies only to develop, our code is not directly depend on that e.g.: unit tests, CoffeeScript to JavaScript transpilation, minification, ... 

  ii) If you really want to install development packages in that case, you can set the dev configuration option to true, possibly from the command line as:

     npm install nodemon --save -dev

  ** If we install any package as global then it will be available for every folder(whole machine).

  NodeMon install: npm install nodemon --save -dev /  npm i nodemon --global

 ** Nodemon helps to automatic restart the server.

 Package versioning & updating
 -----------------------------
 Most of the packages of npm follow the semantic version notation which menas their version numbers always express with three numbers.

 Ex:  "express": "^1.3.4" (major_version. minor_version. patch_version)

    4  --> patch version only intended to fix bug (Minor fix) and its optional

    3 --> minor version introduce some new features in package but it does not include breaking changes. Its recommended bug fix (security purpose)

    1 --> major version is for any new release which can have breaking changes. So if "express: 2.0.0" comes then maybe our code no longer work. We have to very careful about this update. If we write our code in previous version and update the package, then our code will break.

   ^ --> This means we can not change the major version( here 1). Other minor and patch version we can install. Bascially it's <2.0.0.

   ~ (~^1.3.4) --> Here we acn only update patch version. 

   ** Check outdated package: npm outdated

   ** Update package: npm update "package_name"

   ** Delete package: npm uninstall "package_name"

   ** Whenever we share our ocde, then we shouldn't upload the node module folder. To get back all the dependencies in another computer or folder, we need to run this command :  "npm install" 

  Modules in Node js
  ------------------
   Modules are the blocks of encapsulated code that communicate with an external application on the basis of their related functionality.

   Modules can be a single file or a collection of multiple files/folders.

   There are mainly three types of modules:

   Core modules: 

   Node.js has many built-in modules that are part of the platform and come with Node.js installation. These modules can be loaded into the program by using the required function.

   Ex: const http = require('http');

   Local modules:  these modules are created locally into the system by user.

   Third-party Modules: Third-party modules are modules that are available online using the Node Package Manager(NPM).

   Ex: express, moongoose, npm install express

   Ex:
   --
   Math.js

   const { builtinModules } = require("module");

   function add(a,b){
     return a+b;
   }

   function sub(a,b){
     return a-b;
    }

   exports.mul=(a,b)=>a*b  

   module.exports={add,sub}  // exporting function

   index.js

   const {add,sub}=require("./Math")  // import module

   console.log("Value is", add(2,3)); // using this function

   console.log("Value is", sub(10,5));

   
** In node js, every single file treated as a module. 

   const nm= "Sapta"

   modules.exports=nm  --> here name should be same.

   Now import into another file:

   const name=(./First_file); --> here we can use any name but if export multiple, then need to use same name

   console.log(name)
  
  **** If we add type: "module" in package.json file, then we can use import and export keyword in node js.


  what happens when we require() a module
  ---------------------------------------
  Resolving & Loading --> Wrapping --> Execution --> Returning exports --> Caching

  After the module is loaded , the moduler's code is wrapped into a special function which will give us access to a couple of special objects. At node js runtime, it takes the code of our module and puts it inside the immediately invoked function expression. It passes all the mentioned objects into that function and that's why in every module, we automatically have access to stuff like require function. 

  (function(exports, require, module, __filename, __dirname){  --> IIFE function
   // Module code lives here
  }); 

Require function returns "exports" of the required module. module.exports is the returned object. The modules are cached after the first time they are loaded. 



   

   Node js Advantage
   -----------------
   i) Single threaded, based on event driven, non blocking I/O model.   --> always mention these 3 points

   ii)  Perfect for build fast and scalable data-intensive apps.

  iii) Javascript across the entire stack: faster and more efficient development
 
  iv) NPM: huge library of open-source packages available for everyone for free.

  Use of Noe js
  -------------
  i) API with database behind it (preferably NoSQL)

  ii) Data streaming(Ex: youtube)

  iii) Real time chat application

  iv) Server side web application

** We don't use node js for applications with heavy server-side processing (CPU-intensive). Eg: Image manipulation, video processing etc. Mainly we will prefer node js for I/O intensive work.





Read & write data into files
----------------------------
const fs= require('fs');  

here fs stands for File system. By using this module we will get access to functions for reading and writing data to the file system. herw this returns a object with lot of functions which we can use. 

"readFileSync()" function is used to read file synchronously. 

Syntax: readFileSync("file path", "character encoding") // we can have different types of file(like binary, txt,video), so we need to use encoding

Ex:
--
const fs=require('fs');

const textRead=fs.readFileSync('./textFiles/input.txt','utf-8') --> "." mention the current directory where our program starts (here "index.js") 

console.log(textRead); // Output will be the text file content


write in file:

const textWrite=`Hello world, ${textRead}`;

fs.writeFileSync('./textFiles/input.txt',textWrite);

console.log("Written");


Asynchronous nature:

const fs=require('fs');

const textRead=fs.readFile('./textFiles/input.txt','utf-8',(err,data)=>{   // read async
console.log(data);
})

console.log(textRead);

console.log("Hi");

** In synchronous case, its return the result to a variable. But in Asynchronous. it's expect a callback function which gives a error and result. 

fs.writeFile('./textFiles/input.txt',"Hello world async",(err)=>{})  // write async

** appendFileSync() is used to append data in file and it will not overwrite the current data.

Ex:

fs.appendFileSync('./textFiles/input.txt',`Hello this is append data`);

fs.unlinkSync('./textFiles/input.txt'); --> its is used to delete the file.


Blocking and Non Blocking in Node js
------------------------------------
Blocking:
-------
It refers to the blocking of further operation until the current operation finishes. Blocking methods are executed synchronously. 

Synchronously means that the program is executed line by line. The program waits until the called function or the operation returns.

   Blocking ----> Synchronous



Ex:

const fs = require('fs');

const filepath = 'text.txt';

// Reads a file in a synchronous and blocking way 
const data = fs.readFileSync(filepath, {encoding: 'utf8'});

// Prints the content of file
console.log(data);

// This section calculates the sum of numbers from 1 to 10
let sum = 0;
for(let i=1; i<=10; i++){
	sum = sum + i;
}

// Prints the sum
console.log('Sum: ', sum);

Output:

This is from text file.
Sum:  55

Non Blocking
------------
It refers to the program that does not block the execution of further operations. Non-Blocking methods are executed asynchronously. Whenever we get a request from client, the single thread will send the request to someone else, the current thread will not be busy working with that request. 

Asynchronously means that the program may not necessarily execute line by line. The program calls the function and move to the next operation and does not wait for it to return. Once the work is done, a callback function that we register before is called to handle the result. 

   Non Blokcing ----> Asynchronous

Ex:
--

const fs = require('fs');

const filepath = 'text.txt';

// Reads a file in a asynchronous and non-blocking way 
fs.readFile(filepath, {encoding: 'utf8'}, (err, data) => {
	// Prints the content of file
	console.log(data);
});


// This section calculates the sum of numbers from 1 to 10
let sum = 0;
for(let i=1; i<=10; i++){
	sum = sum + i;
}

// Prints the sum
console.log('Sum: ', sum);

Output:

Sum:  55
This is from text file.

** here when the file is finally completely read, then the callback function will be called and the data that was read will then be printed in the console.

why we use callback so many times in Node js
--------------------------------------------
Node js only uses single thread. The thread is a set of instruction that runs on the computer's CPU. Bascially the thread is where our code is executed in machine's processor. 

Node js is single threaded and for each application, there's only one thread. So whenever the users interacting with the application, the code that is run for each user will be executed all in the same thread at the same place. So if one user blocks the the thread with synchronous code, then all user have to wait until execution to finish and it will be a problem for more users. 

To avoid this situation, we need to use asynchronous code in node js. This asynchronous operation will continue in background and other user can do their operation. Once the process complete, the callback function will call to be executed in the main single thread in order to process data.

This is why we use so many callback functions in Node.js .

** When we use callback in our code, that doesn't make it asynchronous automatically. It only works this way for some functions in Node API, such as readFile function.   


How Node js works
-----------------
In Node js architecture, the flow start from client. At first a client sent a request to node server.

     Client  ----- Request  ------> Server(Node server)

Whenever a request comes to server, its goes to event queue in Node Js. The requests are add in queue in Event Queue. Then this request will go to event loop(which is single threaded) from event queue. Event loop is a machine or loop which continious watch on the event queue and if any request is there in event queue, the it takes that request in FIFO (First In First Out) manner and process it. Event loop works continiously.

The request that is picked up from queue, that mainly two types, blocking and non blocking. If it's non blocking process, the event loop process it and return the response to user. 

If its blocking operation, then to resolve this operation, its goes to thread pool. Thread pool is a pool which contains all the threads and these threades are responsible to fulfil the blocking operations. So if any thread is availbale then its assign the blocking operation to that thread and whenever its finish, then thread will come back to thread pool and return the result and that result will go to user. 

    Client  -- Request--> Event Queue --> Event loop --> Non Blocking --> Process --> Return response to user

    
    Client  -- Request--> Event Queue --> Event loop --> Blocking  --> Go for thread --> thread pool --> return result --> execute callback --> Return response to user


** Threades are limited, by default the number of threades are 4. So if blocking operations are come more than 4 , then it has to wait to finish the previous one. So if we use more synchronous code in node js, then user has to wait for long time , as threades are limited. So we should always use asynchronous code. 

** Node js uses libUV, a special library which provides a concept of non blocking I/O. LibUV built in C language which uses system kernal and kernal has multiple threades. So in Node js we are not using multiple threades but behind the scene, our kernal is implementing multiple threades. So the workers who are doing async operations in node js are bascially threades. So when we send a request to server, it will be single threaded but behind the scenes they are using multiple threades and that's what node js fast and flexible.  

Ex of node js Architecture
--------------------------


Features of Event Loop:
----------------------
i) An event loop is an endless loop, which waits for tasks, executes them, and then sleeps until it receives more tasks.

ii) The event loop executes tasks from the event queue only when the call stack is empty i.e. there is no ongoing task.

iii) The event loop allows us to use callbacks and promises.

iv) The event loop executes the tasks starting from the oldest first.


Ex:
--
//Sync
const fs=require('fs');

console.log("1");

fs.writeFileSync('./textFiles/input.txt',"Hello world");

const textRead=fs.readFileSync('./textFiles/input.txt','utf-8')

 console.log(textRead);

 console.log("2");

 Output:

 1
 Hello world
 2

//Async

const fs=require('fs');

console.log("1");

fs.readFile('./textFiles/input.txt','utf-8',(err,data)=>{
console.log(data);
})

 console.log("2");


 Output:

 1
 2
 Hello world

** Non blokcing operations process in asynchronous nature and retunrs the result through callback function.

 
Deep Dive (How internally works)
--------------------------------
Node JS mainly built by two components,   --->      V8 engine & LibUV.

** V8 is a crome engine which executes the javascript code.It converts the javascript code into machine code that a computer can actually understand. It's built by c++ and JS.

** LibUV is a open source library with a strong focus of asynchronous I/O, which implements the concept of Event loop and thread pool. This concepts help to run the async code and run our program efficiently on a single thread. 

internal
-------
Whenever we run a command in Node JS, then at first node js creates a process. This node js process runs on a single thread which is called main thread.In this main thread, below steps are processed one by one.

 i) init project (project initialize)

 ii) Execute top level code (top level code are codes, which directly written on script file, not written in any inside of a function)

     Ex:
     
     console.log("Hello world) // top level code

     const a=10;  // top level code

 iii) Require modules

      Ex: const fs=require('fs');  

 iv) Event callback register (Ex: socket callbacks, event emitters like server.onClose etc)

 v) Start Event loop 

 These all steps are happening in main thread which is a single thread. Besides we also have a thread pool which contains multiple threades. Any CPU intensive task (File system, Crypto etc. related work) is done by thread. By default thread pool size 4 and it can go upto 128 threads.

Now Whenever event loop starts, it offloads all CPU intensive works to threads. 

Event loop execution process
----------------------------
i) Expired timer callbacks (1st it's run in event loop)

   Ex: setTimeout, setInterval etc.

   // promise callback runs

ii) IO polling (Ex: succes callback of file read)

   // promise callback runs

iii) setImmediate callbacks 

   // promise callback runs

iv) close callbacks

    Ex: server close callback, socket close callback etc.

Now after these steps, event loop checks is there any pending task? 

 if its NO, then we exit from program. But if yes  ----> then event loop will start from 1st step.


** When run promise callback?
   -------------------------
   When event loop phase transition happens then promise callback runs. Bascially its run between the phase of event loop.

  process.nextTick() and resolve promises run between the phase of event loop. 

  process.nextTick() is a function that we can use when we really need to execute a certain callback right after the current event loop phase. It's a bit similar with setImmediate, with the difference that setImmediate only runs after the I/O callback phase.
     
Diagram:
-------
Node process:

after node index.js run,.........

inside main tharead:

    init project --> Top level code --> Require module --> Event callbacks register --> Start event loop --> CPU intensive work --> thread pool

if no CPU intnsive work then its enter to event loop

inside event loop:

   Expired timer callback --> IO polling --> setImmediate callback --> close callback --> wrok pending (Yes) --> Expired timer callback

   Expired timer callback --> IO polling --> setImmediate callback --> close callback --> wrok pending (No) --> Exit




Exmaples:
--------
const fs=require('fs');

setTimeout(()=>console.log("Hello from timer 1"),0);

console.log("Hello world");

Output:

Hello world
Hello from timer 1

** 1st here execute top level code then check the expired timer callbacks.

Ex 2:

const fs=require('fs');

setTimeout(()=>console.log("Hello from timer 1"),0);

setImmediate(()=>console.log("Hello from immediate fn 1"));

Output:

Hello from immediate fn 1
Hello from timer 1

** Here order is different. If we run the following script which is not within an I/O cycle(i.e. the main module), the order in which the two timer are executed is non-deterministic, as it is bound by the performance of process.

**** The event loop actually wait for stuff to happen in the poll phase. So in that phase where the I/O callbacks are handled. So when the queue of callbacks is empty, then the event loop will wait in this phase until there is an expired timer. But if we scheduled a callback using setImmediate, then that callback will actually be executed right away after the polling phase and even before expired timer, if there is one. 

So in this case the timer expires right away after zero second but the event loop actually waits and pause the polling phase. So the setImmediate executed first.

Ex 3:

const fs=require('fs');

setTimeout(()=>console.log("Hello from timer 1"),0);

setImmediate(()=>console.log("Hello from immediate fn 1"));

fs.readFile('./textFiles/input.txt','utf-8',()=>{
    console.log("IO polling finished");

    setTimeout(()=>console.log("Hello from timer 2"),0);
    setTimeout(()=>console.log("Hello from timer 3"),5000);
    setImmediate(()=>console.log("Hello from immediate fn 2")); // it will run first

    process.nextTick(()=>console.log("Process nextTick executed"))
})

console.log("Hello from top level");  --> 1st it will run then enter the event loop

Output:

Hello from top level
Hello from timer 1
Hello from immediate fn 1
IO polling finished
Process nextTick executed
Hello from immediate fn 2
Hello from timer 2
Hello from timer 3

** Whenever event loop starts, it offloads all CPU intensive works to threads. We can run 4 threads simultaneously because by default thread sized is 4.

   Also we can change the thread size and increase working capacity.

    process.env.UV_THREADPOOL_SIZE=10;

  now 10 threads work simultaneously as we increase the thread size.

Node JS vs Other programming language
-------------------------------------
Whenever the requests come from client, Node JS runs all the requests on its single threaded event loop. If there's any CPU intensive work, then it's offload to thread pool.

Lanugage like PHP, there is no concept of event loop. Its a multithreaded language. It creates individual thread for every request and there is a strong possibility that at one particular time, all threades are busy and some user has to wait for some time.  

Ex:
 By way of analogy, think of NodeJS as a waiter taking the customer orders while the I/O chefs prepare them in the kitchen. Other systems have multiple chefs, who take a customers order, prepare the meal, clear the table and only then attend to the next customer.


**How, in general, does Node.js handle 10,000 concurrent requests?
------------------------------------------------------------------

Node js works flowchart
----------------------
user do an action
       │
       v
 application start processing action
   └──> make database request
          └──> do nothing until request completes
 request complete
   └──> send result to user

In this scenario, the software spend most of its running time using 0% CPU time waiting for the database to return.

Multithread request:

request ──> spawn thread
              └──> wait for database request
                     └──> answer request
request ──> spawn thread
              └──> wait for database request
                     └──> answer request
request ──> spawn thread
              └──> wait for database request
                     └──> answer request

So the thread spend most of their time using 0% CPU waiting for the database to return data. While doing so they have had to allocate the memory required for a thread which includes a completely separate program stack for each thread etc. Also, they would have to start a thread which while is not as expensive as starting a full process is still not exactly cheap.

Event driven program
--------------------
Node.js makes extensive use of events which is one of the reasons behind its speed when compared to other similar technologies. 

Event-driven programming is used to synchronize the occurrence of multiple events and to make the program as simple as possible. Basically it means as soon as Node starts its server, it simply initiates its variables, declares functions and then simply waits for event to occur and when an event occurs, then we execute the function or logic, so we are executing logic based on certain event.

Node is a event driven model. When some event happens, the we schedule a callback or function call. these functions, setTimeout, setImmediate etc. help to exploite event driven approach. 

How Event-driven architecture works:
-----------------------------------
In node, there are certain object called event emitters that emit named event as soon as something important happens in the app like request hitting server or timer expiring etc. Then these events can then be picked up by event listerners that we developer set up, which will fire off callback functions that are attached to each listner. So on one hand, we have event emitters and on the other hand we have event listners that will react to emitted events by calling callback function. 

    Event Emitter --> emit events --> Event Listner --> calls --> Attached callback function

This event emitter logic is called Observer pattern 



The basic components of an Event-Driven Program are:

i) A callback function ( called an event handler) is called when an event is triggered.

ii) An event loop that listens for event triggers and calls the corresponding event handler for that event.

** Evenet emitter generates the events. In javascript button is a one kind of event emitter. i Node, we use Eventemitter function.

    Ex:
    --
    const Eventemitter= require ('events')  // here we use capital "E" because its a class

    const event= new Eventemitter();

    myEmitter.on("newSale",()=>{
     console.log("There is a new sale")
    })

   myEmitter.on("newSale",(stock)=>{
     console.log(`The are only this ${stock} available`)
   })

   myEmitter.emit('newSale',10);

   Output:

   There is a new sale
   The are only this 10 available

  Stream
  ------
  Stream is used to process(read & write) data piece by piece (chunks), without completing the whole data read or write operation and therefore we don't have to keep all the data in memory to do this operation. 

For Example, when we read a file using streams, we read part of data, do something with it, and then free our memeory and repeat this until the entire file has been processed. Ex: Youtube, Netflix

Advantage:
--------
i) Stream is helpful to handle large volumes of data, Ex: videos.

ii) More efficient data processing in terms of memory(no need to keep all data in memory) and time (we don't have to wait until all the data is available) 

In Node JS, there are 4 fundemental streams:

Readable stream(imp), Writable stream(imp), Duplex stream, Transform stream.

Readable stream:

Readable stream are the ones from which we can read data. Ex: http requests, fs read streams

the data tha comes in when an http server gets a request is actually a readable stream. So all the data in request that comes in piece by piece and not in one large piece. 

** Streams are actually instance of the EventEmitter class. Meaning all streams can emit and listen to named events

Most imp events in Readable stream are "data" and "end". The data event is emitted when there is a new piece of data to consume and the end event is emitted  as soon as there is no more data to consume. 

Most imp functions in Readable stream are "pipe()" and "read()". pipe() function allows us to plug stream together, passing data from one stream to another without having to worry much about events at all. 

Writable stream:

Writable stream are the ones from which we can write data. Ex: http response, fs write streams.

When we have to send data than we need write somewhere and that somewhere is a writable stream. 

Most imp events in writable stream are "drain" and "finish" and Most imp functions are "write()" and "end()"

Duplex stream:

Streams that are both readable and writable at the same time. Ex: web socket (its a bascially communication channel between client and server that works in both directions and stays open once the connection has been established)

Transform stream:

It's a duplex streams that transform data. Ex: zlib Gzip creation

** The mentioned streams all are Consume streams.

Ex:
--
const myServer=require('http').createServer();

myServer.on('request',(req,res)=>{
  const readable=fs.createReadStream('./textFiles/input.txt')
  readable.on('data',(chunk)=>{
    res.write(chunk) // res going to client piece by piece
  })
  readable.on('end',()=>{
    res.end() // this method signal that no more data available to writable stream
  })
  readable.on('error',(err)=>{
    console.log(err);
    res.statusCode(500);
    res.end("File not found")
  })
})

myServer.listen(8000,()=>{console.log("Server running")})

** Our readable stream is much much faster than writable stream. This will overwhelm the response stream which cannot handle all the incoming data so fast. This is called backpressure. To overcome this issue we need to use pipe(). Pipe operator is available in all readable stream and it allows us to pipe the output of a readable stream right into the input of a writable stream.This will handle the backpressure problem. 

const myServer=require('http').createServer();

myServer.on('request',(req,res)=>{
  const readable=fs.createReadStream('./textFiles/input.txt')

  readable.pipe(res); // readableSource.pipe(writableDestination)
})

myServer.listen(8000,()=>{console.log("Server running")})  



2) Create first server
   -------------------

   const http=require('http'); // its a built-in package of node js. Using this we can create our own server

   const myServer=http.createServer((req,res)=>{

        console.log("New request received");
        res.end("Hello from server");   // sent response from server
   })

  server.listen(8000,()=>console.log("Server started));
   

createServer() function creates a server for us. It takes callback function named requestListner which processed our incoming request. Whenever a incoming request comes to our server, it runs the callback function.

We need a port number to run the server. We will listen our server over the port for incoming request. We can no multiple server on a same port.

  One port --> One server

Whenever we sent a request , that request goes to server and then it's run the createServer callback function and then it's send response "Hello from server". 

Ex: store our request in a file as logs

  const fs=require('fs');

  const http=require('http');

  const myServer=http.createServer((req,res)=>{
    const log=`${Date.now()}:New request received\n`
    fs.appendFile('./textFiles/input.txt',log,(err)=>{
      res.end("Hello from server");

    })
  
  })

3) Handling URL in Node
   --------------------
   URL stands for Uniform Resource Locator. 

   URL: https://www.google.com/  --> here https is protocol. Protocal is a set of rules which define how to communicate with server.

    https : hypertext transfer protocol secure. It is secured and encrypted using TLS or SSL protocol.

    www.google.com: Domain which is a user friendly name of IP address of our server. 

    DNS stands for Domain Name server which are special server that are bascially like the phone books of internet. When we enter the URL and request, then   browser makes a request to DNS and this special server then simply match the web address that we typed into the browser to the server's real IP address. 

   

  "/" means path ( Home page or root path).

  "/product/1" --> this is called nested path.

  "/about?userId=1&name="Sapta" --> this is called query parameters.

  ** Query parameters are the extra info which we can pass in URL.

  Ex 1:
  ----
  const http=require('http'); 
  const myServer=http.createServer((req,res)=>{
    console.log(req.url);
    if (req.url=='/about'){
      res.end("Hello this is Sapta")  --> this will print for url: localhost:8080/about
    }
    else{
      res.end("Hello from server");
    }
  
  })

  ** An HTTP Header is a piece of information about the response that we are send back. 

  Ex 2:
  ----
  const url=require('url');

  const http=require('http'); 
  const myServer=http.createServer((req,res)=>{
    console.log(req.url);
    const {query,pathname}=url.parse(req.url,true);  // it's parse the URL, if we pass true then it creates object of query parameter
    console.log(query.userId);   // 1
    console.log(query.name);     // sapta
  
    if (req.url==='/'){
      res.end("Hello from server")
    }
    else if (req.url==='/about'){
      res.end("This is Sapta");
    }
    else{
      res.writeHead(404,{
        'Content-type': 'text/html'
      });
      res.end('<h1>404 not found</h1>');
    }
  
  })

  here we are passing this URL: http://localhost:8000/about?name=sapta&userId=1

4) HTTP Methods
   ------------
   There are several methods like : GET, POST, PUT, PATCH DELETE

   GET: It is used when we want to get some data from server

   POST: It is used when we want to send and mutate some data in server

   PUT: It is mainly used when we upload some file in server. It means put some data to server. Here we send entire updated object.

   PATCH: it is used to change existing data to server. Here we send part of updated object

   DELETE: it is used to delete  data from server





Node Note 2.....................................................................



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

   The key abstraction of information in REST is a resource and therefore all the data that we want to share in the APi should be divided in to logical resources. Resource is a object or representation of something, which has some data associated to it.  

ii) The resources should be exposed using structured, resource-based URLs.

iii) To perform different action of data, API should use right http methods. 

iv) send data as json(usually)

    JSON is a very lightweight data interchange format which is heavily used by web APIs. In JSON, all the keys have to be string. 

v) API must be stateless.

   A REST API's all states is handled on the client and not on the server. This means that each request must contain all the info necessary to process a certain request. The server should not have to remember previous requests.  State refers to piece of data that might change over time. 

** REST API works on Server Client Architecture. It means Server and Client are different entity and it should not be dependent on each other. Suppose we send data in html then it won't be worked in any mobile application. So in REST API, this shouldn't be happened. Mostly we send data as JSON format. 

** We should always respect all http methods.Suppose we want to update data, then we should use PATCH menthod and shouldn't use other method. 

** Postman is a utility tool which helps in API testing and documentation. 

    
