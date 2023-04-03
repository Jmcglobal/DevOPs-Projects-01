## LEMP STACK IMPLEMENTATION
A web stack is a combination of software components that are used to create a web application. It includes the operating system, web server, programming language, database, and web framework. A web stack is also known as a “stack” or a “solution stack”.

The operating system is responsible for running the web server, which is responsible for processing requests from the web browser. The programming language is used to create the application code. The database stores data that the application needs to access, and the web framework is responsible for handling requests and generating responses.

The combination of these components allows a web application to be developed quickly and efficiently. By selecting the right combination of components, web developers can build applications that are powerful, secure, and performant.
# Components of web stack include
- Operating System
- Web Server
- Databases and 
- Programming language
# Lemp Stack is made of "Linux, Nginx, Mysql, and PHP, Python or Perl.
To implement those stack, i need virtual machine with linux operating system installed, in this scenerio, i will use Amazon EC2 instance.
# EC2-Instance with linux Operating System
![linux-ec2](https://user-images.githubusercontent.com/101070055/229382903-00b064a8-0c83-4bd5-b09c-68c211cb6f05.png)
- Details of my deployed EC2.
![Ec2-Os](https://user-images.githubusercontent.com/101070055/229382938-d79cbf12-1f07-4225-82cc-fbf1f3ec29ed.png)

On my LAMP Stack implementation on project-1, i explained the details on how to access EC2 instance with local terminal
- Access-key, the key pair used when creating the instance.
- Security group, I allowed SSh port 22 open in order to access the machine using SSH protocol.
![local-access](https://user-images.githubusercontent.com/101070055/229383095-f78036bb-a3f6-4264-815d-d1efb85de7bb.png)
# Web-server installation (Nginx)
- Nginx is a web server that is used to serve web content to users. It is a high-performance, open-source, and lightweight web server that is used to serve static and dynamic content. It is also used as a reverse proxy, load balancer, and HTTP cache. Nginx is known for its scalability, reliability, and low resource consumption.
- sudo apt update -y
- sudo apt install nginx -y

To confirm if the Nginx Server is active and running.
- systemctl status nginx

![nginx-server-active](https://user-images.githubusercontent.com/101070055/229383324-118be3eb-98be-443d-b31f-6a64bbfde4bd.png)

From the picture i can confirm i have successfully installed the nginx webserver.
For users and I to access the content that is available on this server, i will go ahead and open default HTTP port 80. The EC2 have security group attached to it, i will edit the inbound rule to allow HTTP protocol on port 80 from any IP address.
![sg-http-rules](https://user-images.githubusercontent.com/101070055/229383474-8cc12cb2-b111-41d0-8309-e47313d46ad4.png)
Now i have opened the HTTP port 80, i will use the EC2 public IP address and paste it on web browser.
I can as well use this command to display my EC2 instance public iP; curl -s http://169.254.169.254/latest/meta-data/public-ipv4
Curl Command to access the web content from my terminal;
- curl http://localhost

![Nginx-successful](https://user-images.githubusercontent.com/101070055/229383573-767510cf-0748-4bdb-a0c7-8cf4f46ac324.png)
Wow !!!, Its successfull.

# MYSQL installation and configuration.
MySQL is a powerful and popular open source relational database management system (RDBMS) that is used to store, organize, and retrieve data for websites and applications. It is fast, reliable, secure, and cost-effective, making it an ideal choice for web-based applications

- sudo apt install mysql-server -y
- sudo mysql >> to login .

![mysql-installed](https://user-images.githubusercontent.com/101070055/229385376-e907b76b-f88a-4af6-bb50-c21038a5853a.png)

Sudo mysql is a command that allows the user to access the MySQL database as the root user. It allows the user to execute commands in the MySQL database with the highest privileges, granting the user access to all databases and tables, as well as the ability to add, delete, or change user privileges. It’s recommended that we run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to my database system, but before running the script i will need to change the password.

ALTER USER 'root'@'localhost'IDENTIFIED WITH mysql_native_password BY 'jmcglobal'; using mysql_native_password as default authentication method. We’re defining this user’s password as jmcglobal.
![mysql-alter-user](https://user-images.githubusercontent.com/101070055/229385745-fbb353b6-dd25-45e2-92db-74001fcdef81.png)

- mysql> exit - To exit out of the database

I will Start the interactive script to secure the database

- sudo mysql_secure_installation
NOTE: This will ask for password Authentication first, enter password and hit enter,
![mysql-secure](https://user-images.githubusercontent.com/101070055/229386461-80932cd9-8c94-4b05-a9ff-55a4271d8978.png)

Answer Y for yes, or anything else to continue without enabling the key depends on your choice though, either you enter yes or no, therefore i follow the pattern and enable the best security pattern i want.

It will ask you to remove the anonymous users, press y Disallow root login remotely, press y Remove test database and access to it, press y Reload privilege tables now, press y - then that will be all, the database is now secured.

![remove-mysql-access](https://user-images.githubusercontent.com/101070055/229386550-45459107-6fc5-42e5-9f14-2c60c86843f0.png)

I will test if am able to log in to the MYSQL console:

- sudo mysql -p > -p flag will prompt for the password used after changing the root user password
Now MYSQL server is now installed and secured.
