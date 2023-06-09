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

This will be quick and simple for demonstration,

I will use AWS ubuntu EC2 instance for this, and most the commands i will use here,

Update and upgrade ubuntu instance

            sudo apt update -y && sudo apt upgrade -y

Add certificates

             sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates

             curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

Install NodeJS

            sudo apt install nodejs -y

Install MongoDB MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For my example application, I am adding book records to MongoDB that contain book name, isbn number, author, and number of pages.

            sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

            echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" |
            sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
            
Install MongoDB

            sudo apt install -y mongodb

Start The Server

            sudo service mongodb start

Verify that the service is up and running

            sudo systemctl status mongodb

Install npm -Node package manager

            sudo apt install npm -y
            sudo npm install body-parser

CCreate a folder named ‘Books’

            mkdir Books && cd Books

In the Books directory, Initialize npm project

            npm init
            
Add a file to it named server.js

            vi server.js

Copy and paste the web server code below into the server.js file.

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

# Install Express And Set UP Routes To The Server

Install Express and set up routes to the server

            sudo npm install express mongoose
            
In ‘Books’ folder, create a folder named apps

            mkdir apps && cd apps
            
Create a file named routes.js

            vi routes.js
            
Copy and paste the code below into routes.js

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
            
In the ‘apps’ folder, create a folder named models

            mkdir models && cd models

Create a file named book.js

            vi book.js

Copy and paste the code below into ‘book.js’

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
            
 Access the routes with, AngularJS provides a web framework for creating dynamic views in your web applications. In this tutorial, we use AngularJS to connect our web page with Express and perform actions on our book register.
 
Change the directory back to ‘Books’

cd ../..
Create a folder named public

mkdir public && cd public
Add a file named script.js

vi script.js
Copy and paste the Code below (controller configuration defined) into the script.js file.

var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});
In public folder, create a file named index.html;

vi index.html
Copy and paste the code below into index.html file.

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
Change the directory back up to Books

cd ..
Start the server by running this command:

node server.js
The server is now up and running, we can connect it via port 3300. You can launch a separate Putty or SSH console to test what curl command returns locally.

curl -s http://localhost:3300
It shall return an HTML page, it is hardly readable in the CLI, but we can also try and access it from the Internet.

For this – you need to open TCP port 3300 in your AWS Web Console for your EC2 Instance.

You are supposed to know how to do it, if you have forgotten – refer to Project 1 (Step 1 — Installing Apache and Updating the Firewall)

Your Sercurity group shall look like this:

Now you can access our Book Register web application from the Internet with a browser using Public IP address or Public DNS name.

Quick reminder how to get your server’s Public IP and public DNS name:

You can find it in your AWS web console in EC2 details

Run curl -s http://169.254.169.254/latest/meta-data/public-ipv4 for Public IP address or
curl -s http://169.254.169.254/latest/meta-data/public-hostname for Public DNS name.

Test the page using the server public IP


For any information, am available on,

Emial: mmadubugwuchibuife@gmail.com

Not Core software Engineer, but >> I'm Skilled  Cloud Architect/DevOps Engineer Specialist .
