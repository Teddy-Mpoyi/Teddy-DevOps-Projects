# MERN STACK PROJECT
## MERN Stack Definition
### MERN stack is a web development framework. It consists of MongoDB, Express.js, React.js, and Node.js as its working components.

Each of these 4 powerful technologies provides an end-to-end framework for the developers to work within and each of these technologies play a big part in the development of web applications.

Here are the details of what each of these components is used for in developing a web application when using MERN stack:

MongoDB: A document-oriented, No-SQL database used to store the application data.

Node.js: The JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.

Express.js: A framework layered on top of NodeJS, used to build the Backend of a site using NodeJS functions and structures. Since NodeJS was not developed to make websites but rather run JavaScript on a machine, ExpressJS was developed.

React.js: A library created by Facebook. It is used to build the User Interface or UI components that create the user interface of the single page web application.

## Understanding the Parts of the MERN Stack

### MongoDB

- MongoDB is a cross-platform document-based database known as a nonrelational document-orientated database, or NoSQL. MongoDB stores data in flexible documents using a query language based on JavaScript Object Notation (JSON). The size, number and content of document fields vary, so the data structure can be altered later on. MongoDB is easy to scale and quite flexible, making it popular among many web developers.

### Express.js

- Express is a back end framework for Node.js web applications. We can use Express to make writing server code much simpler, as opposed to the traditional alternative of manually writing full web server code on Node.js, since it lets us avoid having to repeat the same code like we would using the Node.js HTTP module. Express’s framework was designed to enable simple, easy construction of APIs and efficient web applications. It is loaded with plugin features and is renowned for its speed. Its minimalistic structure makes it simple for developers to pick up and start using it right away.

### React.js

- In React, views are declarative, meaning developers do not have to manage the effects of changes in data or the view’s state. (A view’s state is the object that determines the behavior of individual components). Instead, React lets developers use a full-featured programming language to design conditional or repetitive Document Object Model or DOM elements rather than relying on templates that automatically create repetitive DOM or HTML elements. React lets the same code run in the browser and server simultaneously, which is what enables React to anchor the MERN stack and makes it essential to the operation.

### Node.js

- Node.js is a cross-platform JavaScript runtime environment that is built on Chrome’s V8 JavaScript engine and is intended to allow developers to build scalable network applications and execute JavaScript code outside of the browsers. Node.js works by using its own module system that is based on CommonJS, meaning it does not need an enclosing HTML page to put together more than one JavaScript file.

How does the MERN stack work?

The MERN architecture enables us to easily build a 3-tier web architecture i.e the Frontend, Backend, and Database layers entirely using JavaScript and JSON.

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/17b0627f-250b-4c5e-b8e1-08fb25b551bc)

Basically In this project, you are tasked to implement a web solution based on a MERN stack in AWS Cloud.

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/efa3677c-ca2b-4ab8-8f51-c90e052f3460)

## Step 0
Hint #1: When you create your EC2 Instances, you can add Tag “Name” to it with a value that corresponds to a current project you are working on - it will be reflected in the name column of the EC2 Instance. Like this:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/b52f3455-3fe0-4ab9-8a6d-102721b4d2d5)

**Next**, download and launch MobaxTerm, create a new SSH session with , ‘ubuntu’ as username and your private key (.pem file) that you downloaded to local machine at the EC2 creation like this:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/1bfbacda-82bb-4ba5-85dd-517afc5d87a1)

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/64846fa1-407b-4866-b871-e128acccd252)

**Just as a reminder In this project, we are required to implement a web solution based on a MERN stack hosted on a Linux server. We will be deploying an application that is used to create ‘Todo’ lists by implementing the steps below:**

## Step 1 - Prepare the backend Linux server
Update and upgrade ubuntu using the following commands:
~~~
$ sudo apt update
$ sudo apt upgrade
~~~
## Step 2 - First, we will need to locate and install Node.js on the server.
Lets get the location of Node.js software from the Ubuntu repositories.
~~~
curl -fsSL https://deb.nodesource.com/setup_21.x | sudo -E bash - &&\
sudo apt-get install -y nodejs
~~~
## Step 3 - Install NodeJS on the server

Install Node.js with the command below
~~~
sudo apt-get install -y nodejs
~~~
![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/f5641372-19cf-49cc-9d3c-9e56f670ef7e)

**Note:** The command above will install both nodejs and npm. NPM is a package manager. Similar to what apt does on Ubuntu, NPM is used to install node modules/software and manages dependency conflicts intelligently.
Verify the node installation with both commands below:
~~~
$ node -v

$ npm -v
~~~
We'll see something like this:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/4651fac8-4c37-4539-8ff7-c6f43ca0aee9)

## Step 4 - Application Code Set

**Create a new directory for your To-Do project:
~~~
$ mkdir Todo
~~~
**Verify that the Todo directory is created
~~~
lS
~~~
We'll see something like this:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/3094a2c8-1161-4964-b9f9-07b539b5eb6f)

**Now change your current directory to the newly created one:
~~~
cd Todo
~~~
![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/01af5e38-42cb-4674-99df-65de93587791)

**Next, we will use the command `npm init` to initialise our project, so that a new file named `package.json` will be created. This file will normally contain information about our app and the dependencies that it needs to run. Follow the prompts after running the command. We can hit enter to accept the default values, then we accept to write out the `package.json` file by typing yes.
~~~
$ npm init
~~~
We'll see something like this:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/67b1e2e9-c013-4487-a3da-7d266d3890e1)

**Run the command ls to confirm that you have package.json file created.

## Step 5 - Install ExpressJS

Express is a Backend framework for Node.js web applications. it lets us avoid having to repeat the same code like we would using the Node.js HTTP module. 
Remember that Express is a framework for Nodes.js, therefore a lot of things developers would have programmed is already taken care of out of the box.
Therefore it simplifies development and abstracts a lot of low level details. For example, Express helps to define routes of our application based on HTTP methods and URLs.

Inside the Todo list directory, We will install Express using npm:
~~~
$ npm install express
~~~
Now create a file index.js with the command below
~~~
$ touch index.js
~~~
Run ls to confirm that your index.js file is successfully created
~~~
$ ls
~~~
Inside the todo list directory, We will Install the dotenv module
~~~
$ npm install dotenv
~~~
We'll see something like this:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/64e902ba-be65-4e91-abb3-67e36318736a)

We need to open the index.js file with the command below
~~~
vim index.js
~~~
**Type the code below into it and save. Do not get overwhelmed by the code you see. For now, simply paste the code into the file.
~~~
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
~~~
**We'll see something like this
NOTE we have specified to use port `5000` in the code. This will be required later when we go on the browser.
To test if our server works. We open our terminal in the same Todo directory as our index.js file and type:
~~~
$ node index.js
~~~
If every thing goes well, We should see our Server running on port 5000 in our terminal.

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/7ff88618-98b0-41fc-803f-1d0710713ba5)

Now we need to open this port in EC2 Security Groups. Refer to Project 1 Step 1 – Installing the Nginx Web Server. There we created an inbound rule to open TCP port `80`, you 
need to do the same for port `5000`, like this:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/d491eb2d-807e-4de5-8cbd-b27ce23874aa)

Let's open up our web browser and try to access our server's Public IP or Public DNS name followed by port 5000
~~~
http://<PublicIP-or-PublicDNS>:5000
~~~
Quick reminder how to get your server’s Public IP and public DNS name:
~~~
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
curl -s http://169.254.169.254/latest/meta-data/public-hostname
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/a4502395-74d0-4202-a38c-20e132185218)

## Step 6 - Routes

There are three actions that our To-Do application needs to be able to do:
1. Create a new task
2. Display list of all tasks
3. Delete a completed task

Each task will be associated with some particular endpoint and will use different standard **HTTP request methods: POST, GET, DELETE**

For each task, we need to create `routes` that will define various endpoints that the `To-do` app will depend on. So let us create a folder `routes`
~~~
mkdir routes
~~~

**TIP:** You can open multiple shells in Putty or Linux/Mac to connect to the same EC2

Change directory to `routes` folder
~~~
cd routes
~~~
Now, create a file `api.js` with the command below
~~~
touch api.js
~~~
Open the file with the command below
~~~
vim api.js
~~~
Copy below code in the file. (Do not be overwehelmed with the code)
~~~
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {
});

router.post{'/todos', (req, res, next) => {
});

router.delete('/todos/:id', (req, res, next) => {
})

module.exports = router;
~~~
## Step 7 - Models

Since the app is going to make use of Mongodb which is a NoSql database, we need to create a model which will be used to define the database schema. 
A model is at the heart of JavaScript based applications, and it is what makes it interactive.
This is important so that we will be able to define the fields stored in each Mongodb document.

In essence the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database.
These are known as ***virtual properties***. To create a Schema and a model, install ***mongoose*** which is a Node.js package that makes working with mongodb easier.

Switch directory back Todo folder with `cd ..` and install Mongoose
~~~
$ npm install mongoose
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/2ca66109-35c4-47a4-96e0-215a3f2a0b5f)

Create a new folder with `mkdir models` command
~~~
$ mkdir models
~~~
Change directory into the newly created `‘models’` folder with
~~~
$ cd models
~~~
Inside the models folder, create a file and name it
~~~
$ touch todo.js
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/9ee9b9d3-d866-46a3-9e78-cf5d95291c24)

All three commands above could have beeen defined in a single line to be executed consequently with the help of `&&` operator, like this:
~~~
mkdir models && cd models && touch todo.js
~~~
Open the file created with `vim todo.js` then paste the code below into the file:
~~~
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/dc66c9ad-0850-4d21-be3b-61c041c55a52)

Now we need to update our routes from the file `api.js` located in the 'Routes' directory to make use of the new model.

Within the Routes directory, open api.js with `vim api.js`, delete the code from within with the `:%d` command and paste the code below into it then save and exit
~~~
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/54f4cd5d-7bde-4c30-b4cb-c5f103ada0a3)

## Step 7- MongoDB Database

We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB 
database as a service solution (DBaaS), so to make life easy, you will need to sign up for a shared clusters free account, 
which is ideal for our use case.

Head over https://www.mongodb.com/atlas-signup-from-mlab to follow the sign up process, select AWS as the cloud provider, and choose a region near you.

Create a mongodb database and connection inside mLab and then obtain the connection string through the connect TAB. 
Also, update the password and database name and removed the <>.

`DB=mongodb+srv://mpoyitendayi:<password>@cluster0.a0ffuul.mongodb.net/?retryWrites=true&w=majority`

Create a file in your TODO directory with and name it .env. Add the connection string to access the database in it, just as below

In order to allow the Node.js to connect to the database in Mongodb, the `index.js` was updated so as to reflect the use of dotenv.
~~~
touch .env
vim .env
~~~

Create a mongodb database and connection inside mLab and then obtain the connection string through the connect TAB. 
Also, update the password and database name and remove the <>.

In the `index.js` file, we specified `process.env` to access environment variables, now we need to update the `index.js` to reflect the use of `.env` so that Node.js can connect to the database.

Simply delete existing content in the file, and update it with the entire code below.

To do that using `vim`, follow below steps

Open the file with `vim index.js`

Press `esc`

Type `:`

Type `%d`

Hit ‘Enter’

The entire content will be deleted, then,

Press `i` to enter the insert mode in `vim`

Now, paste the entire code below into the file.
~~~
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
~~~
Using environment variables to store information is considered more secure and best practice to separate configuration
and secret data from the application, instead of writing connection strings directly inside the `index.js` application file.
Start your server using the command:
~~~
node index.js
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/918dcd95-5dff-47db-a1c0-0a71ab1d0702)

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/f2c42810-c7a5-4ec3-ab47-c9ebff170d75)

## Step 8 -Testing Backend Code without Frontend using RESTful API

So far we have written our TODO application, and configured backend database, but we do not have a frontend UI yet.
We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTful API. 
Therefore, we will need to make use of some api development clients to test our code.

In this project, we will use `Postman` to test our API so we will need to download and install it on our local machine. 
We should test all the API endpoints and make sure they are working. 
For the endpoints that require body, we should send JSON back with the necessary fields since it's what we setup in our code.

Install Postman in Ubuntu
~~~
$ sudo snap install postman
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/7540e532-e1d4-4ca5-a666-033ef6f75368)

Now we open our Postman, We will create a `POST` and `GET` requests in postman to the API

**Note:***After downloading and installing Postman on your local machine, issue the command "node index.js" so that the application can connect to the database and keeps the connection open until you finish testing your api!!*

Do not abort your connection until your are testing your APIs!!

http://localhost:5000/api/todos

`Post REquest`

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/465f9f44-ad81-453d-b403-cf8a58ee961d)

`Get Request`

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/a7aaa74c-45b3-4e15-b64e-45f3634c0bb3)

## Step 9 - Frontend creation

Since we are done with the fucntionality we want from our Backend and API, it is time to create a user interface for a Web client (browser) to 
interact with the application via API. To start out with the Frontend of the To-do app, we will use the `create-react-app` command to scaffold 
our app.

In the sae root directory as our Backend code, which is the Todo directory, run:
~~~
npx create-react-app client
~~~
This will create a new folder in our `Todo` directory called `client`, where we will add all the react code.

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/068e3bc4-e14e-4a3c-91b0-f39db97c7821)

## Step 10 - Running the React App

Before testing the react app, there are a number of dependencies that need to be installed.
We need to Install **concurrently:** is used to run more than one command simultaneously from the same terminal window.
~~~
$ npm install concurrently --save-dev
~~~
![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/71b11907-04a9-4143-91f9-2c50a263a34b)

We need to Install **nodemon**: It is used to run and monitor the server. If there is any change in the server code,
nodemon will restart it automatically and load the new changes.
~~~
$ npm install nodemon --save-dev
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/428daf07-43d2-419a-b0b9-826e2915f7cc)

Next, in the `Todo` folder, open the `package.json` file. Change the highlighted part of the below screenshot and replace with the code that follows.

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/083c5b31-4513-4d52-9ce5-a6c0e1759bcc)

~~~
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
~~~
As shown below:

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/ada6068f-d06f-4acd-bb11-1a3665d6f62e)

## Step 11 - Configure Proxy in `package.json`

Enter into the `client` folder from the Todo directory
~~~
cd client
~~~
Open the `package.json` file
~~~
vi package.json
~~~
Add the key value pair in the package.json file "proxy": "http://localhost:5000".

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/9cba2328-67b9-4369-a2ad-398f068c7f1e)

The whole purpose of adding the proxy configuration in number 3 above is to make it possible to
access the application directly from the browser by simply calling the server url like `http://localhost:5000` rather than
always including the entire path like `http://localhost:5000/api/todos`

Now, we will ensure that we are inside the `Todo` directory, and simply do:
~~~
$  npm run dev
~~~

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/b1656d14-b5c1-4e3a-b2a1-fff63754928b)

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/8e3ac720-5416-48a6-a345-4b904566b801)

Our app should open and start running on `localhost:3000`

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/aef56cbc-1125-4834-93ea-efc94a1e3720)

Important note: In order to be able to access the application from the Internet we have to open TCP port 3000 on EC2 by adding a new Security Group rule. 
we already know how to do that. Additionally me must ensure that we use the **public IP** address of our EC2 instance in the of localhost ie. `54.234.12.16:3000`

## Step 12 - Creating our React Components

One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. 
For our Todo app, there will be two stateful components and one stateless component. From our Todo directory we run:
~~~
cd client
~~~
move to the `src` directory
~~~
cd src
~~~
Inside your `src` folder create another folder called `components`
~~~
mkdir components
~~~
Inside `components` directory create three files `Input.js`, `ListTodo.js` and `Todo.js`.
~~~
touch Input.js ListTodo.js Todo.js
~~~
Open `Input.js` file
~~~
vi Input.js
~~~
Copy nd paste the following
~~~
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input
~~~
Axios is a promise based HTTP client for the browser and Node.js, We need to cd into our client from our terminal
and run yarn add axios or npm install axios.

We move to the src folder
~~~
cd ..
~~~
we move to the clients folder
~~~
cd ..
~~~
Install Axios in the clients folder
~~~
$ npm install axios
~~~
Go to components directory
~~~
$ cd src/components
~~~
After that open your `ListTodo.js`
~~~
$ vi ListTodo.js
~~~
In the `ListTodo.js` copy and paste the following code
~~~
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo
~~~
Then in your `Todo.js` file we write the following code
~~~
import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;
~~~

We need to make little adjustment to our react code. Delete the logo and adjust our `App.js` to look like this.

Move to the src folder
~~~
cd src
~~~
Make sure that you are in the src folder and run
~~~
vi App.js
~~~
Copy and paste the code below into it after deleting the existing code
~~~
import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;
~~~
After pasting, exit the editor.

In the src directory open the `App.css`
~~~
vi App.css
~~~
First delete the existing code, then copy and paste the following code into `App.css`
~~~
.App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}

input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}

input:focus {
outline: none;
}

button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}

button:focus {
outline: none;
}

ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}

li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}

@media only screen and (min-width: 300px) {
.App {
width: 80%;
}

input {
width: 100%
}

button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}

@media only screen and (min-width: 640px) {
.App {
width: 60%;
}

input {
width: 50%;
}

button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}
~~~
**Exit**
In the src directory open the `index.css` file
~~~
vim index.css
~~~
Ensure to delete any existing code before copying and pasting the code below:
~~~
body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
~~~
Go to the Todo directory
~~~
cd ../..
~~~
When you are in the Todo directory run:
~~~
npm run dev
~~~
Assuming no errors when saving all these files, our To-Do app should be ready and fully functional with the 
functionality discussed earlier: creating a task, deleting a task and viewing all your tasks !!

![image](https://github.com/Teddy-Mpoyi/Teddy-DevOps-Projects/assets/103863428/7fba2803-1fd4-45f2-ad08-8873aa88fe0e)
























































