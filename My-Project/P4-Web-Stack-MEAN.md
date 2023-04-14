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

Install MongoDB MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.

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

