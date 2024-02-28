# MEAN-STACK
## What is a MEAN Stack
**MEAN stands for:** 
* MongoDB 
* Express.js 
* AngularJS 
* Node.js. 

**MEAN is an end-to-end JavaScript stack largely used for cloud-ready applications. MEAN is a full-stack development toolkit which is also refer to as a collection of JavaScript technologies used to develop web applications from the client to the server and from server to database, everything is based on JavaScript. It is also free and open-source stack which offers a quick and organized method for creating rapid prototypes for web-based applications.**


## MEAN STACK is comprised of four different technologies:
* **M**ongoDB (Document database): Stores and retrieve data.
* **E**xpress JS (Back-end application framework): Makes requests to Database and return a response
* **A**ngularJS (Front-end application framework): Handles Client and Server Requests
* **N**ode.js (JavaScript runtime environment): Accept requests and display results to end user


![image](https://user-images.githubusercontent.com/85270361/129397413-e9948fc7-f02c-4ff2-a527-bab8e9072f41.png)


**There are variations to the MEAN stack such as MERN (replacing Angular.js with React.js) and MEVN (using Vue.js). The MEAN stack is one of the most popular technology concepts for building web applications.**

# How Does the MEAN Stack Work?

## MEAN Stack Architecture
The MEAN architecture is designed to make building web applications in JavaScript and handling JSON incredibly easy.

![image](https://user-images.githubusercontent.com/85270361/129397323-0d4d8820-0aa0-4476-ab59-fc96e700df2e.png)


# MEAN Stack Components

## MongoDB

**MongoDB** is an open source, NoSQL database designed for cloud applications. It uses object-oriented organization instead of a relational model. It stores data in a key-value pair format, using binary data type like JSON. MongoDB is an ideal choice for a database system where we need to manage large sized tables with millions of data. Moreover, including a field to MongoDB is easier as it does not require updating the entire table. With MongoDB we can develop an entire application with just one application which is JavaScript.


## Express

**Express** is a mature, flexible, lightweight server framework. It is designed for building single, multi-page, and hybrid web applications. This lightweight framework uses the Pug engine to provide support for templates. Express is a web application framework for Node.js. It handles all the interactions between the frontend and the database, ensuring a smooth transfer of data to the end user.


## Angular

**Angular** JS is an open-source JavaScript framework and it is maintained by Google. The goal of this framework is to introduce MVC(Model View Controller) architecture in the browser-based application that makes the development and testing process easier. The framework helps us create a smarter web app that supports personalization.
AngularJS allows us to use HTML as a template language. Therefore, we can extend HTML's syntax to express the components of our application. Angular features like dependency injection and data binding eliminate plenty of code that we need to write.


## Node.js

**Node.js** is an open source JavaScript framework that uses asynchronous events to process multiple connections simultaneously. It allows developers to create web servers and build web applications on it. It's a server-side Javascript execution environment and also an ideal framework for a cloud-based application, as it can effortlessly scale requests on demand.
Node.js uses a non-blocking and event-driven I/O model. This makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices. We’re likely to find Node.js behind most well-known web presences.


![image](https://user-images.githubusercontent.com/85270361/129399038-47e15f55-ba45-4b6c-9e1d-252416e48476.png)


# MEAN STACK USE CASES

**While the MEAN stack isn’t perfect for every application, there are many uses where it excels. It’s a strong choice for developing cloud native applications because of its scalability and its ability to manage concurrent users. The AngularJS frontend framework also makes it ideal for developing single-page applications that serve all information and functionality on a single page. Here are a few examples for using MEAN:**

* Todo and calendar applications
* Expense tracking applications
* News aggregation sites
* Mapping and location finding


![image](https://user-images.githubusercontent.com/85270361/129399664-96bdd361-8839-4893-a685-9a68b32be97d.png)


## In this Project we are going to deploy a web based application on our Linux server using MEAN STACK by following the steps outlined below:

# Step 0 – Preparing prerequisites

In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.

If you do not have an AWS account – go back to Project 1 Step 0 to sign in to AWS free tier account ans create a new EC2 Instance of t2.nano family with Ubuntu Server 20.04 LTS (HVM) image.


# Step1: Install NodeJs

## Task
In this assignment you are going to implement a simple Book Register web form using MEAN stack.

## We will install nodejs on our server

Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js is used in this tutorial to set up the Express routes and AngularJS controllers.

**Update ubuntu**

```
sudo apt update
```

**Upgrade ubuntu**

```
sudo apt upgrade
```

**Install NodeJS**

```
sudo apt install -y nodejs
```

# Step 2: Install MongoDB

**MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.**

* **Run these commands:**

From a terminal, install gnupg and curl if they are not already available:

```
sudo apt-get install gnupg curl
```

**OUTPUT:**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/b2f6ae64-52bc-4af8-924d-01f962ecc477)

**To import the MongoDB public GPG key, run the following command:**

```
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
```

**OUTPUT:**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/992cd923-855e-4e99-994c-69f7f9991584)

**Create a list file for MongoDB**

~~~
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
~~~

**OUTPUT:**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/f898d51c-e0a4-4944-86fe-544ab01f0909)

**Reload local package database**

~~~
sudo apt-get update
~~~

**OUTPUT:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/a27d8193-005f-40b0-b9d8-37336b970b76)

**Install the MongoDB packages**

```
sudo apt-get install -y mongodb-org
```

**OUTPUT:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/8356f08c-51ce-4094-91e9-5ca2018d2aae)

**Start the Server**

**Init System**

To run and manage your mongod process, you will be using your operating system's built-in init system. Recent versions of Linux tend to use systemd (which uses the **systemctl command**), while older versions of Linux tend to use **System V init** (which uses the service command).

If you are unsure which init system your platform uses, run the following command:

```
ps --no-headers -o comm 1
```

**OUTPUT:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/ecf8dd16-bc3c-4776-ab57-586640d72c45)

```
sudo systemctl start mongod

```

**Verify that the service is up and running**

```
sudo systemctl status mongod

```

**OUTPUT:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/8ea3f832-463c-4d84-ae94-f5fca2e1fb32)

## Install [npm](https://www.npmjs.com) – Node package manager.

```
sudo apt install -y npm
```

## Install ‘body-parser package

We need ‘body-parser’ package to help us process JSON files passed in requests to the server.

```
sudo npm install body-parser
```

**OUTPUT:**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/57fa6885-e434-4330-99b6-8ba819f3e972)

**Create a folder named ‘Books’**

```
mkdir Books && cd Books
```

**In the Books directory, Initialize npm project**

```
npm init
```

**OUTOUT:**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/7fb29437-2e1a-44ec-b8d7-78d6f0aab778)
![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/31665312-2b69-4cff-a2bf-c0b877ce24cb)

**Add a file to it named server.js**

```
touch server.js
```

```
vi server.js
```

**Copy and paste the web server code below into the server.js file.**

```
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});
```

**OUTPUT:**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/13a20750-c42f-423d-baa4-26c9db1005db)

# Step 3: Install Express and set up routes to the server

**Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. We will use Express to pass in book information to and from our MongoDB database.**

**We also will use Mongoose package which provides a straight-forward, schema-based solution to model your application data. We will use Mongoose to establish a schema for the database to store data of our book register.**


```
sudo npm install express mongoose
```

**OUTPUT:**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/813b0769-6c2a-4f1a-bd49-33551136664a)
![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/ae7bcfab-f11e-4b1b-a1e4-e22f0e0a7b11)

**In ‘Books’ folder, create a folder named apps**

```
mkdir apps && cd apps
```

**Create a file named routes.js**


```
touch routes.js
```

```
vi routes.js
```

**Copy and paste the code below into routes.js**

```
var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};
```

**OUTPUT:**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/1f545d0b-51e9-454b-8696-bb5b8cb9d3b6)

**In the ‘apps’ folder, create a folder named models**

```
mkdir models && cd models
```

**Create a file named book.js**

```
touch book.js
```

```
vi book.js
```

**OUTPUT:**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/86353a0f-1388-4a1a-8e5f-f71fe8f62a01)

**Copy and paste the code below into ‘book.js’**

```
var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/a4cc395f-9ad6-4238-b905-b0917e0151b0)

# Step 4: Access the routes with AngularJS

AngularJS provides a web framework for creating dynamic views in your web applications. In this tutorial, we use AngularJS to connect our web page with Express and perform actions on our book register.

**Change the directory back to ‘Books’**

```
cd ../..
```

**Create a folder named public**

```
mkdir public && cd public
```

**Add a file named script.js**

```
touch script.js
```

```
vi script.js
```

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/c127371a-2ac9-4b49-91b5-b79abae1eba6)

**Copy and paste the Code below (controller configuration defined) into the script.js file.**

```
var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};
```

**OUTPUT:**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/7be09529-2622-4de4-b19b-a5a3439feda5)

**In public folder, create a file named index.html;**

```
touch index.html
```

```
vi index.html
```

**Copy and paste the code below into index.html file.**

```
<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>

        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>
```

**OUTPUT:**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/6413b0a8-fcf5-489a-9232-b1221f285112)

**Change the directory back up to Books**

```
cd ..
```

**Start the server by running this command:**

```
node server.js
```

**OUTPUT:**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/bf73bb84-6fa4-4ba7-a138-c4e3328acdc2)

## Note the server is now up and running, we can connect into it via port 3300. You can launch a separate Putty or SSH console to test what curl command returns locally.

```
curl -s http://localhost:3300
```

It shall return an HTML page, it is hardly readable in the CLI, but we can also try and access it from the Internet.

For this – you need to open TCP port 3300 in your AWS Web Console for your EC2 Instance.

You are supposed to know how to do it, if you have forgotten – refer to Project 1 (Step 1 — Installing Apache and Updating the Firewall)

Your Security group shall look like this:


**OUTPUT:**

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/a211f77d-f1bf-49e0-a855-582dd1d12396)

Now you can access our Book Register web application from the Internet with a browser using Public IP address or Public DNS name.

Quick reminder how to get your server’s Public IP and public DNS name:

You can find it in your AWS web console in EC2 details

**Run `curl -s http://169.254.169.254/latest/meta-data/public-ipv4` for Public IP address or
`curl -s http://169.254.169.254/latest/meta-data/public-hostname` for Public DNS name.**

This is how your Web Book Register Application will look like in the browser:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/80f38685-988a-4eb8-af7d-ee99678fef3b)
