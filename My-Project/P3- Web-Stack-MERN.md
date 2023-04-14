![mern-stack](https://user-images.githubusercontent.com/101070055/231606941-17854b4e-703f-4aed-87c1-aee340571715.jpeg)

## WHAT IS MERN STACK ?
MERN stack is a combination of technologies used to develop web applications. It stands for MongoDB, Express, React, and Node.js. These technologies provide a great technical foundation for developers to build web applications with a highly secure front-end and robust back-end. MongoDB is a NoSQL database used for storing data, Express is a web application framework for Node.js, React is a JavaScript library for building user interfaces, and Node.js is a JavaScript runtime environment used for executing JavaScript code on the server side.

# MERN STACK OVERVIEW.

![mern-1](https://user-images.githubusercontent.com/101070055/231607040-02f91b8a-3faf-45eb-854d-665302cfe748.png)

MongoDB: MongoDB is a document-oriented NoSQL database used for storing data in JSON-like documents. It is a cross-platform, open-source database that provides high performance, high availability, and automatic scaling.

Express: Express is a web application framework for Node.js. It is used for building web applications and APIs. It provides a robust set of features for web and mobile applications.

React: React is a JavaScript library for building user interfaces. It is used for creating reusable UI components. It is used for developing single-page applications and mobile applications.

Node.js: Node.js is a JavaScript runtime environment used for building server-side applications. It is used for developing fast and scalable network applications.

These four technologies are used together to create modern web applications. MongoDB is used for storing data, Express is used for building the backend, React is used for building the frontend, and Node.js is used for running the application.

## I will implement a solution base of MERN stack in AWS Cloud.
![mern-03](https://user-images.githubusercontent.com/101070055/231607825-d65e7cf6-c91c-4e1d-acd0-c0ef2b7cfb78.png)

![ubuntu-machine](https://user-images.githubusercontent.com/101070055/231736726-2d0d2bc1-4a0d-4180-81b8-e01f6cc3cf8e.png)


- I am using AWS EC2 ubuntu instance for this project.
    - 2 vCPU and 2G memery
    - key pair
    - security group
    - 30 Gib volume
    - AZ us-east-1a

# Backend Configuration.

- Update and Upgrade Ubuntu Machine.
     - sudo apt update -y .
     - sudo apt upgrade -y .

- Get the location of Node.js software from Ubuntu repositories.
    - curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -

- install Node.js .
    
    - sudo apt-get install nodejs -y .

- The command will install nodejs and npm,"npm is a package manager for Node like apt for Ubuntu, it is used to install node Modules & packages and manage dependency conflicts.

- Confirm Node installation
   
   - node -v
    - npm -v

![node-version](https://user-images.githubusercontent.com/101070055/231740030-9a0bc623-64d8-4ea2-9a24-be1b278768cc.png)

- Make a directory for the project  .
    
    - mkdir Mern-Stack
    - cd Mern-Stack
    - ls -alh

![project-directory](https://user-images.githubusercontent.com/101070055/231746041-d4650f57-6ccb-465c-a219-8eece8fb3f52.png)

- Run npm command to initialise the project, hit enter to accept default values to the end.
   
   - npm init .
- ls command to confirm package.json have initialise .
    
    - ls

![npm-init](https://user-images.githubusercontent.com/101070055/231747075-65bafe91-64d2-44a4-b1a0-eb33dbcad500.png)

# Install Expressjs .

    - npm install express
    
- create index.js file .
     
     - touch index.js

- Install dotenv module.
    
    - npm install dotenv

- Edit index.js with vim editor . - sample dummy commands

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

NOTE:
  i specified to use port 5000 in the code, so that the application will reachable
  I have also open the port 5000 on the security group attached to the ubuntu machine.
- save and exit
   
   - :wq

- Start the server
    
    -  node index.js
    
![running-server](https://user-images.githubusercontent.com/101070055/231762821-498eb96a-d5c9-4390-9559-dda1715af29c.png)

- To access the app via browser
        
        - https://ubuntu-machine-public-ip:5000
        
- terminal command to display Machine DNS & Public IP .
        
        - curl -s  http://169.254.169.254/latest/meta-data/public-ipv4 
        - curl -s http://169.254.169.254/latest/meta-data/public-hostname 

NOTE: Ensure port 5000 is allow on the security group inbound rule entry attached to the ubuntu machine.

# Routes
There are three actions that the  application needs to be able to do:
        
        Create a new task
        Display list of all tasks
        Delete a completed task
        
Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.
For each task, i need to create routes that will define various endpoints that the  app will depend on.

- create routes directory and change directory inside routes directory
       
       - mkdir routes && cd routes

- Create app.js file inside the routes directory and use vim editor to edit the file
       
       - touch app.js
       - vim app.js
       
- Paste the sample code inside the file.
            const express = require ('express');
            const router = express.Router();

            router.get('/todos', (req, res, next) => {

            });

            router.post('/todos', (req, res, next) => {

            });

            router.delete('/todos/:id', (req, res, next) => {

            })

            module.exports = router;

# Create Models
What is Models ?
A model is at the heart of JavaScript based applications, and it is what makes it interactive.
  
Using a model in MERN stack development gives developers the ability to quickly and easily create complex data structures and retrieve data from a database. Models also provide a layer of abstraction between the view layer and the data layer, making it easier to manage the data and maintain the code. Models also provide a way to ensure that data is consistent across multiple sources and that it is stored and retrieved securely. Finally, models allow developers to easily create relationships between data entities and perform complex queries on the data.

        - To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier.
- Install mongoose .

        - npm install mongoose

- Create model folder and change directory into the model folder.
        
        - mkdir models && cd models

- Create a file js file inside the models.
       
       - touch todo.js

- use vim editor to open todo.js.
        
        - vim todo.js

- Enter this smaple dummy code 
           
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

Now i need to update my routes from the file api.js in ‘routes’ directory to make use of the new model.

- Change directory to routes and use vim editor to open app.js, then enter sample  dummy code
       
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
        
 
 

# MONGODB CONNECTION.

I need a database where i will store  data. For this i will make use of Mongodb Atlas. mongodb Atlas provides MongoDB database as a service solution, so to make life easy, i will need to sign up for a shared clusters free account, which is ideal for my use case.
You can sign up by clicking https://www.mongodb.com/atlas-signup-from-mlab .

- After signing up, login in with your email and pasword ..
- click database, click Build Database

![build-db](https://user-images.githubusercontent.com/101070055/231814323-e0e03d94-9d6b-4848-bf6d-e4ea3c30f75e.png)

- Select M0 which is free tier, select AWS as provider and select closest region, then click create.

![m0-free](https://user-images.githubusercontent.com/101070055/231814823-b8159d80-73d5-4cb5-a3b9-41be637ba85c.png)

- click on (Database Access) , then click on add new database user.

![db-user](https://user-images.githubusercontent.com/101070055/231817433-585e6764-6c0d-41c7-b313-c877785c5ad8.png)

- click on network access to add IP entry that will acess the database

![network-access](https://user-images.githubusercontent.com/101070055/231818419-3ccbee91-93b5-44d8-a0c4-ea2f505f4bbf.png)

- select Allow Access From Anywhere, then click confirm 

![anywhere](https://user-images.githubusercontent.com/101070055/231819187-e287f1de-ed61-40cd-a904-28fb559e3e74.png)

In the index.js file, i specified process.env to access environment variables, but i have not yet created this file. So i need to do that now.

Create a file in your mern-stack directory and name it .env and use vi to edit the file

        touch .env
        vi .env
        
Add Connection Stringe to access the database

        DB = 'mongodb+srv://<username>:<password>@cluster0.mqxyjjj.mongodb.net/?retryWrites=true&w=majority'

Use esc to exit text input, type :wq to save and exit.

- Where to see database connection stringe

        click on Database
        click on connect 
        
   ![click-db](https://user-images.githubusercontent.com/101070055/231826434-4487ca18-628a-46af-997a-44a578ffffa2.png)
     
        Click on Drivers, Under connect to your application
       
   ![connect](https://user-images.githubusercontent.com/101070055/231826808-1cdbcf43-74a9-4861-bd2c-d024921bc7df.png)

        scroll down and copy the connection URL
        
   Now i need to update the index.js to reflect the use of .env so that Node.js can connect to the database.
   
   - Use vim editor to edit index.js, delete entire codes and use below dummy code
   
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

        
Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the index.js application file.

- Start The Server again
        node index.js
        
If successfull .....

![db-running](https://user-images.githubusercontent.com/101070055/231835865-07ffed53-ed51-4c36-94df-f1aee9c88ffe.png)

Testing Backend Code without Frontend using RESTful API, backend part of the application have been written, and configured a database, but no frontend UI yet. Therefore i will use a POSTMAN API software the backend.

        Post Request 
        Get request

Create a GET request to your API on http://:5000/api/todos. This request retrieves all existing records from out To-do application (backend requests these records from the database and sends it us back as a response to GET request).

Set Headers as follows

![header-api](https://user-images.githubusercontent.com/101070055/231883688-22187176-c479-4bbe-aab2-1cfe74eff840.png)


![post-api](https://user-images.githubusercontent.com/101070055/231880053-ca582ad0-1735-448f-9b37-aa7f8201792c.png)


![get-api](https://user-images.githubusercontent.com/101070055/231881309-a696fa98-1312-475e-acad-f48ffdf428a6.png)

Database .

![db-items](https://user-images.githubusercontent.com/101070055/231882753-bf3e149c-465f-48a2-b213-14a1a81c9acc.png)



# CREATE FRONTEND

I have don the functionality i want from the backend and API, now it is time to create a user interface for a web client to interact with the application via API, i will use the create-react-app command ..

- Run React command

            npx create-react-app client

This will create a new folder in project directory called client, where i will add all the react code.

Running a React App Before testing the react app, there are some dependencies that need to be installed.

- Install concurrently. It is used to run more than one command simultaneously from the same terminal window

            npm install concurrently --save-dev

- Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it       automatically and load the new changes.

            npm install nodemon --save-dev

- Edit package.json file on project director, add below entry on the  scripts...

            "scripts": {
            "start": "node index.js",
            "start-watch": "nodemon index.js",
            "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
            },

![pkg json](https://user-images.githubusercontent.com/101070055/231892257-53c71e15-c23e-45bf-84bc-2bd92bba9ceb.png)

- Start the app on project directory with command:
            npm run dev

My app will open and start running on port 3000
To ensure the app is reachable, i edited the security , added another allow rule entry for port 3000.

![app-run](https://user-images.githubusercontent.com/101070055/231894723-23a6d18d-8f65-4521-a713-9b074416982d.png)

To access the app on my browser

            http://localhost:3000
 
![reacr-run](https://user-images.githubusercontent.com/101070055/231894958-01dc6a9e-0c25-4a1a-b9ce-2460fb52d55f.png)

The react app is up and running

Creating React Components One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For my app, there will be two stateful components and one stateless component. From my project directory run.

            cd client

Change to src directory

            cd src

Create new directory called components inside src dir 

            mkdir components

Move into the components directory

            cd components

Create three files inside the components directory, Input.js, ListTodo.js, and Todo.js

            touch Input.js ListTodo.js Todo.js

use vim editor open Input.js file

            vim Input.js

Enter the sample dummy code 

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


To make use of Axios, which is a Promise based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.

Change directory to project directory mern-stack

            cd ~/Mern-Stack

Install Axios

            npm install axios

From Project dir, Use vi editor to edit ListTodo.js file on components dir.

            vi client/src/components/ListTodo.js

Enter this sample code

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
            
Edit Todo.js file on components dir.

            vi client/src/components/Todo.js

Enter this sample code

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

Edit App.js on src directory

            vi client/src/App.js

Enter this sample code

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
            
Edit App.css on src dir.

            vi client/src/App.css

Enter this sample code

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
            
Edit index.css on src directory

            vi client/src/index.css

Enter this sample code

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
            
          
Start the project again

            npm run dev
            
![success-mern-stack](https://user-images.githubusercontent.com/101070055/231908115-6e2270d0-8e49-46bd-a131-a6f306da7bc8.png)


And the react App is now functional ..................................

Check Mern-Stack-2nd for another implementation ............................... 

Available on.....

Email: mmadubugwuchibuife@gmail.com
