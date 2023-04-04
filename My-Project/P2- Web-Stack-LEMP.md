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

## PHP INSTALLATION
PHP is a powerfull language, It is a great choice for creating dynamic web pages and applications.
 Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. I’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, I’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases.
 
 - sudo apt install php-fpm php-mysql

I now have PHP components and its dependencies installed.

## Configuring Nginx to use PHP processor
I will create a server blocks (similar to virtual host in Apache) to encapsulate configuration details and host more than one website domain in this single server.
By default Nginx has one server block configured and enabled bu default, and configured to serve web content from '/var/www/html', i will leave this directory and create another directory on the same root directory '/var/www/jmcglobal-websites.

#Creating another directory to serve website content.
- sudo mkdir /var/www/jmcglobal-websites

I will change the ownership from root to user.
- ~$ sudo chown $user:$USER /var/www/jmcglobal-websites

![change-permission](https://user-images.githubusercontent.com/101070055/229642043-ad006519-c85b-4c8d-98a3-6de5781e01cb.png)

I will create a new configuration file in Nginx sites-available directory, I will use nano text editor

- ~$ sudo nano /etc/nginx/sistes-available/jmcglobal-websites

here is the configuration.

![nginx-config](https://user-images.githubusercontent.com/101070055/229642774-61e7c7fe-df43-4e99-88e9-11d7ebd676ec.png)


Here’s what each of these directives and location blocks do:

listen — Defines what port Nginx will listen on. In this case, it will listen on port 80, the default port for HTTP.

root — Defines the document root where the files served by this website are stored.

index — Defines in which order Nginx will prioritize index files for this website. It is a common practice to list index.html files with a higher precedence than index.php files to allow for quickly setting up a maintenance landing page in PHP applications. You can adjust these settings to better suit your application needs.

server_name — Defines which domain names and/or IP addresses this server block should respond for. Point this directive to your server’s domain name or public IP address.

location / — The first location block includes a try_files directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate resource, it will return a 404 error.

location ~ .php$ — This location block handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the php7.4-fpm.sock file, which declares what socket is associated with php-fpm.

location ~ /.ht — The last location block deals with .htaccess files, which Nginx does not process. By adding the deny all directive, if any .htaccess files happen to find their way into the document root ,they will not be served to visitors.

Activate Configuration by linking to the config file from Nginx sites-available directory:
- ~$ sudo ln -s /etc/nginx/sites-available/jmcglobal-websites /etc/nginx/sites-enabled

To test my configuration for syntax errors.
- ~$ sudo nginx -t

output:

![nginx-config-test](https://user-images.githubusercontent.com/101070055/229644402-a9cc022b-5323-4236-98c2-d3e4dfa41079.png)

I also need to disable default Nginx host that is currently configured to listen on port 80.

- ~$ sudo unlink /etc/nginx/sites-enabled/default

To reload Nginx server.

- ~$ sudo systemctl reload nginx

My new website is now active, but the web root /var/www/jmcglobal-websites is still empty. I Create an index.html file in that location so that I can test that the new server block works as expected:

sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP'
$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html

i can now use web browser along with my instance public IP to access this server.

To access it locally,

curl http://localhost

Now my nginx server configuration is successful.
