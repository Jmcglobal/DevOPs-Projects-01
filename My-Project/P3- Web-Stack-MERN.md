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
