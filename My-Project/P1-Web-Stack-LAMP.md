  ## WHAT IS WEB TECHNOLOGY STACK ?
  A web stack is a combination of technologies or components used to create/develop, maintain and deploy web applications.

# COMPONENTS OF WEB STACK.
# - Operating system: 
The Os is most fuundamental piecse of software in a web stack, it provides an interface between software and hardware, allowing application to talk to the hardware. Linux, Windows, and Mac OS are common operating system.
# - A Web Server:
A web server is most important component of a web stack, it is responsible for taking request from a client and sending the HTML, CSS, Javascript and other assets that make up the web page. Web servers include "Apache, and Nginx" Which are most widely used web servers.
# - A database Server:
A database server is used to store information and data. Popular databases include MYSQL, SQL Server, MongoDB, in AWS cloud we have DynamoDB, RDS and Aurora databases.
# - Programming language:
The programming language is the code or language used to create the software application or websites. It interacts with the web server and the database. this language include, PHP, Python, Ruby, Javascript and the rest.

## COMMON WEB TECHNOLOGY STACK
- LAMP : THis is an acronym for Linux, Apache, MYSQL, and PHP, it is web development platform that consist of open-source software used to create dynamic websites and web applications.

## DEPLOYING LAMP STACK

1. I will start first by deploying a virtual machine using AWS Cloud, EC2, which will also have linux operating system installed.
NOTE: When deploying a services using AWS or any public cloud, you need to consider the following,
- Latency,
- Geolocation,
- And Security.
There different regions and within iinside a region there is availability zones, we also need to coonsider this, not all type of services are available in all regions, but the difference is not much, availability zones and regions gives us opporturnity to make our deployment redundant, resillience and highly avaialable against failure. You can choose to run our deployment in different region. 

2. When deploying EC2 virtual machine, it must be inside any of available zones on VPC, it mmust have security group, access-key if you wish to access it remotely, then select instance type, the storage and ram, and some other parameters that are optional.

3. EC2 Virtual Machine up and Running.
![Ec2-up-and-runing](https://user-images.githubusercontent.com/101070055/229311835-29f8949e-01be-4d15-a811-c77aa9336e35.png)
## ACCESS THE EC2 VIRTUAL MACHINE
![access-ec2-server](https://user-images.githubusercontent.com/101070055/229312930-2349aa1b-05c1-479a-8f23-b7595cae4758.png)

4. Access the Virtual machine with SSH protocol using the machine public IP and access-key, but before that, we need to open SSH port 22. This is done by modifying inbound rules on the EC2 security security group, Allow SSH from any IP or range of IP
![Allow-ssh](https://user-images.githubusercontent.com/101070055/229312028-89f48a11-258a-4883-90cc-943e8f647281.png)
On my Linux local terminal " sudo ssh -i <Access-key.pem> ubuntu@<ec2-public-ip>
![access-ec2-termianl](https://user-images.githubusercontent.com/101070055/229312581-d553a5c0-7a33-452e-881c-7923cf29d96a.png)

## Installing Apache Web-Server
  Now i have virtual machine with linux operating system that is running, am going to run update command also and install Apache2 Web- Server
 - sudo apt update -y
 - sudo apt install apache2 -y
 - systemctl status apache2 - This command is used to confirm if the web server is installed and running successfully
  ![apache2-server-up-and-running](https://user-images.githubusercontent.com/101070055/229312839-77e2189a-1342-4de1-bcc4-25dfb1b5bb28.png)
 ## Accessing The Web-Server Using the browser.
  - By default the webserver is listening on HTTP port 80.
  - I need to modify the security group again and allow HTTP traffic on Port 80.
  ![allow-hhtp-traffic](https://user-images.githubusercontent.com/101070055/229313145-db9ac998-1a78-4a6b-a4a3-47c79338465b.png)
 - I can use the server public IP or DNS name to access it via web browser.
![access-apache2-via-browser](https://user-images.githubusercontent.com/101070055/229313260-1cccb50a-9c7b-407e-9d20-c9d874bafc35.png)
I can confirm that the web server is working.
I can as well use curl command to access it locally by running " curl http://localhost", This will fetch the website content and display it on local machne terminal.
  
## INSTALLING MYSQL DATABASE: 
  . MySQL is a popular relational database management system used within PHP environments.
  
  - sudo apt install mysql-server -y
  This command will run and install Mysql
  
  - sudo mysql - To access mysql

  ![accesses-mysql](https://user-images.githubusercontent.com/101070055/229314054-15f09636-feef-458b-aa85-75c70338b429.png)

  
Sudo mysql is a command that allows the user to access the MySQL database as the root user. It allows the user to execute commands in the MySQL database with the highest privileges, granting the user access to all databases and tables, as well as the ability to add, delete, or change user privileges.
It’s recommended that we run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to my database system, but before running the script i will need to change the password.
 
- ALTER USER 'root'@'localhost'IDENTIFIED WITH mysql_native_password BY 'Password1';
   using mysql_native_password as default authentication method. We’re defining this user’s password as PassWord1.
  
- mysql> exit - To exit out of the database
  
 I will Start the interactive script to secure the database
  
 - sudo mysql_secure_installation
 
  NOTE: This will ask for password Authentication first, enter password and hit enter,
  
  ![scrip-mysql-1](https://user-images.githubusercontent.com/101070055/229314196-821f81b2-93c8-4350-9f81-648feef7202f.png)

 Answer Y for yes, or anything else to continue without enabling 
 the key depends on your choice though, either you enter yes or no, therefore i follow the pattern and enable the best security pattern i want.
  
 It will ask you to remove the anonymous users, press y
 Disallow root login remotely, press y
 Remove test database and access to it, press y
 Reload privilege tables now, press y - then that will be all, the database is now secured
 ![mysql-script-run](https://user-images.githubusercontent.com/101070055/229314400-df92df69-9974-4289-81a9-f0ef0faf797c.png)

 I will test if am able to log in to the MYSQL console:
 - sudo mysql -p > -p flag will prompt for the password used after changing the root user password

 Now MYSQL server is now installed and secured.
  
 ## INSTALL PHP TO COMPLETE LAMP STACK IMPLEMENTATION.
  PHP is the component of my setup that will process code to display dynamic content to the end user. In addidtion, i will need php-mysql, a PHP module that allows PHP to communicate with MYSQL-based databases, and also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.
  To install these packages, i will run this code at once
  
 - sudo apt install php libapache2-mod-php php-mysql
 - php -v > To check PHP version and confirm installation
  
  At this point, the LAMP stack implementation is complete
  
To test mytup with a PHP script, it’s best to set up a proper Apache Virtual Host to hold myebsite’s files and folders. Virtual host allows me to ve multiple websites located on a single machine and users of the websites will not even notice it. Its awesome !!

  ## CREATING VIRTUAL HOST-1 FOR WEBSITE USING APACHE
  Apache on ubuntu by default has one server block enabled that is configured to serve document from the '/var/www/html directory. I will leave the configutaion and add my own directory.
  
 # Creating a directory using "mkdir" command:
  ![create-jmcglobal-directory](https://user-images.githubusercontent.com/101070055/229316431-76d5fe8c-7907-4d60-bff0-e5979791f474.png)

 - $ sudo mkdir /var/www/jmcglobal-website
  
 # Change ownership of the directory from root to ubuntu user:
  
 - $ sudo chown -R $USER:$USER /var/www/jmcglobal-website
  ![chown-ownership](https://user-images.githubusercontent.com/101070055/229316519-d173a402-9b8b-47b0-90c1-68a787773c20.png)

 I will use vi or vim text editor to create jmcglobal.conf file inside /etc/apache2/sites-available/
  
 - ~$ vi /etc/apache2/sites-available/jmcglobal.conf
 I have created a new blank file jmcglobal.conf, then i will configure my virtual host to host jmcglobal website contents
  <virtualHost *:80>
    ServerName jmcglobal
    ServerAlias www.jmcglobal-tech.net
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/jmcglobal-website
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log
  </virtualHost>
 ![vi-editor-virtual-hots](https://user-images.githubusercontent.com/101070055/229318417-c06d2b35-d5a2-4139-a5ba-90c44678e966.png)
 
  Then i will save the file by hitting esc button, then type : and input wq then ENTER to save and exit.
 - ~$ ls /etc/apache2/sites-available > to view the list of files inside sites-available apache directory.
  
  With this VirtualHost configuration, am telling Apache to serve jmcglobal website using /var/www/jmcglobal-website as its root directory.
  
  # To enable the new virtualHost.
  - ~$ sudo a2ensite jmcglobal
# To disable default site, i can simply use this command
  - ~$ sudo a2dissite 000-default
# To confirm the file does not contain syntax error.
  - ~$ sudo apache2ctl configtest
# To reload Apache in order my configuration to take effect
  - ~$ sudo systemctl reload apache2

  My new website is now active in another virtualHost, i will create an index.html file inside jmcglobal-website directory to test if the virtual host is working. 
  
- ~$ sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' 
$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/jmcglobal-website/index.html
  
  I can now go to web browser to confirm it bu using the virtual machine public IP address
  
  it worked !!!
  ![website-workee](https://user-images.githubusercontent.com/101070055/229319829-185de727-e3c4-4392-89c1-ba4c736bd077.png)

  ## ENABLING PHP ON THE WEBSITE
  With the default Directory index settings on Apache, index.html file takes first precedence over index.php file.
  To change this behavour, i will edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed.
  
  - ~$ sudo vim /etc/apache2/mods-enabled/dir.conf
  ![enable-php](https://user-images.githubusercontent.com/101070055/229320725-aea272f6-d290-4a64-89e3-342666dffe08.png)
After saving and closing the file, i will reload Apache2
# PHP Script to confirm that Apache is able to handle and process request for PHP files
  - ~$ vim /var/www/jmcglobal-website/index.php > this command will create index,php file and open the blank page for text input,
  Now type this inside the file
  - <?php
   - phpinfor();
Then save and exit, then text again be using the virtual machine public IP address.
![php-working](https://user-images.githubusercontent.com/101070055/229322430-c3479783-9a73-4e0c-8505-75a540da1410.png)

yeah !!! its working.......

      >>>>>>>>>>>>>>>>>>> JMCGLOBAL-TECH>>>>>>>>>>>>>>>>>>>>>>>>>>

