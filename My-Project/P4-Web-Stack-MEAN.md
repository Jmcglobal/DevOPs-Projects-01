# MEAN STACK WEB TECHNOLOGY.

Mean Stack is a full-stack JavaScript solution that is used for the development of web applications. 
It is an open-source collection of software that includes MongoDB, Express.js, AngularJS, and Node.js.

![mean-stacks-main](https://user-images.githubusercontent.com/101070055/232013021-fb08a6b9-bd21-4d47-b3f2-b4218815f93d.jpg)


# M: MongoDB.

MongoDB is a NoSQL database that stores data in JSON-like documents. It is used to store data for web applications.

# E: Expressjs.

Express.js is a web application framework for Node.js. It is used to create web applications and APIs.

# A: AngularJS.

AngularJS is a JavaScript framework for creating single-page web applications. It is used to create interactive user interfaces.

# N: Nodejs.

Node.js is a JavaScript runtime environment. It is used to execute JavaScript code on the server-side.

# Mean Stack Implementation.

Almost i have done similar setups on MERN STACK project, here i will implement Mean stack, I will use AWS EC2 ubuntu instance as the server.

# Commands/dummy codes to set up the project.

SSH  inside the AWS EC2 ubuntu instance and update and upgrade

      sudo apt update -y && sudo apt upgrade -y

Install Nodejs and npm 

      curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
      sudo  apt-get install nodejs -y    

Confirm npm and nodejs installation

      npm -v && node -v
      
Create a backend directory and change dir into the new dir.

      mkdir backend && cd backend
      
Initialise backend nodejs mean project. This will prompt me to enter some basic information, i can as well filled out those information or use a default value, then after it will create a package.json file.

      npm init
      
 Edit package.json file and add, under scripts "start": "node, index.js",
 
  ![edit-packagejson](https://user-images.githubusercontent.com/101070055/232034787-a35f854b-214a-4150-bd39-9fe6b66bbcea.png)
    
      
Create index.js file, 3 dir, and 3 files inside the dir controllers/employee.js, routes/employee.js, models/employee.js.

          mkdir models && mkdir controllers && mkdir routes && touch models/employee.js && touch controllers/employee.js && touch             routes/employee.js
          touch index.js
          
Install extra needed packages.

      npm install body-paser cors express mongoose
      
![dir](https://user-images.githubusercontent.com/101070055/232044917-fa2f383f-2bad-4441-8b4d-17a865fff57a.png)
      

Body-parser

- Body-parser is a middleware for Node.js that parses incoming request bodies in a middleware before the handler, available under the req.body property. It helps to extract the entire body portion of an incoming request stream and exposes it on req.body. This body-parser module parses the JSON, buffer, string and URL encoded data submitted using HTTP POST request. It is typically used to support HTML form elements.

Cors (Cross-origin resource Sharing)

- CORS is used to enable the client-side application (Angular) to communicate with the server-side application (Node.js) without having to worry about cross-domain requests.
- By allowing cross-domain requests, CORS enables the client-side application to access data from the server-side application without the need for the server-side application to explicitly allow the request.

# Create model.
 I will start defining schema for employee collection iside employee.js on models dir, using the below dummy code.
 
          const mongoose = require('mongoose');
          const Schema = mongoose.Schema;
          // Define collection and schema
          let Employee = new Schema({
             name: {
                type: String
             },
             email: {
                type: String
             },
             designation: {
                type: String
             },
             phoneNumber: {
                type: Number
             }
          }, {
             collection: 'employees'
          })
          module.exports = mongoose.model('Employee', Employee) 
 
# Configure Controller.

Controller contains logic to access data from the database using different routes, i will be using mongoose to perform CRUD operation on the MongoDB

controllers > employee.js using the below codes.

            var mongoose = require('mongoose'),
              Employee = mongoose.model('Employee');
            exports.listAllEmployee = function(req, res) {
              Employee.find({}, function(err, Employee) {
                if (err)
                  res.send(err);
                res.json(Employee);
              });
            };
            exports.createEmployee = function(req, res) {
              var emp = new Employee(req.body);
              emp.save(function(err, result) {
                if (err)
                  res.send(err);
                res.json(result);
              });
            };
            exports.readEmployee = function(req, res) {
              Employee.findById(req.params.employeeId, function(err, result) {
                if (err)
                  res.send(err);
                res.json(result);
              });
            };
            exports.updateEmployee = function(req, res) {
              Employee.findOneAndUpdate({_id: req.params.employeeId}, req.body, {new: true}, function(err, result) {
                if (err)
                  res.send(err);
                res.json(result);
              });
            };
            exports.deleteEmployee = function(req, res) {
              Employee.remove({
                _id: req.params.employeeId
              }, function(err, result) {
                if (err)
                  res.send(err);
                res.json({ message: 'Employee successfully deleted' });
              });
            };
            
 # Routes
 
 Routes are the ways to access the data from the database with the help of controllers.
 
- routes > employee.js using the below dummy codes
 
         module.exports = function(app) {
            var employee = require('./../controllers/employee');
            app.route('/')
              .get(employee.listAllEmployee)
            app.route('/employee')
              .get(employee.listAllEmployee)
              .post(employee.createEmployee);
            app.route('/employee/:employeeId')
              .get(employee.readEmployee)
              .put(employee.updateEmployee)
              .delete(employee.deleteEmployee);
        };

# Server

 - index.js , Using the below codes, I will use Mongo Atlass connection string to DB, therefore i have setup 

Create .env file on project dir, then paste connection stringe of mongo Atlass

        DB = 'mongodb+srv://<db-username>:<password>@cluster0.psuakcv.mongodb.net/?retryWrites=true&w=majority'
        
- Configure the Server

          var express = require('express'),
             app = express(),
             port = process.env.PORT || 3000,
             mongoose = require('mongoose'),
             cors = require('cors'),
             bodyParser = require('body-parser'),
             Employee = require('./models/employee'); 
             mongoose.Promise = global.Promise;
             mongoose.connect(process.env.DB,  {   useNewUrlParser: true, useUnifiedTopology: true })
             .then(() => {
                console.log('Database sucessfully connected')
             },
             error => {
                console.log('Database could not connected: ' + error)
             }
          )
          // Setting up configurations
          app.use(bodyParser.json());
          app.use(bodyParser.urlencoded({ extended: false }));
          app.use(cors());
          // Setting up the routes
          var employeeRoute = require('./routes/employee'); 
          //importing route
          employeeRoute(app); 
          //register the route
          app.listen(port, () => {
            console.log('Connected to port ' + port)
          })
          // Find 404 and hand over to error handler
          app.use((req, res, next) => {
             next(createError(404));
          });
          // error handler
          app.use(function (err, req, res, next) {
            console.error(err.message); 
            if (!err.statusCode) err.statusCode = 500; 
            // If err has no specified error code, set error code to 'Internal Server Error (500)'
            res.status(err.statusCode).send(err.message); 
            // All HTTP requests must have a response, so let's send back an error with its status code and message
          });


