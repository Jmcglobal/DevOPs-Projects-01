#               CLIENT-SERVER ARCHITECTURE WITH DATABASE


Client-Server Architecture with Database is a type of distributed computing architecture that consists of two or more computers connected to each other over a network. 
The computers are known as clients and servers. The client is the computer that requests services from the server, while the server is the computer that provides the services.

A database is a collection of data that is organized and stored in a computer system. 
It is used to store and retrieve data from the server.

The diagram below illustrates the client-server architecture with database.

In this architecture, the client computer sends a request to the server computer. 
The server computer then retrieves the requested data from the database and sends it back to the client computer. 
The client computer then processes the data and displays the results.

The client-server architecture with database is a popular architecture for distributed computing applications. 
It is used in many applications such as web applications, enterprise applications, and mobile applications. 
It is also used in many databases such as Oracle, 

![three-tier](https://user-images.githubusercontent.com/101070055/232142792-4712cd03-4b67-4046-b807-967b9cb6692c.png)

This diagram is real scenerio operation of almost 99% of web applications, Mern , Mean, Lamp, Lemp Stack follows the exact operation.

# OVerview

![client](https://user-images.githubusercontent.com/101070055/232145458-0eb8ad01-5217-4619-b436-594d8618d3ac.png)

# Implement A Client Server Architecture Using Mysql Database

Here i will launch 2 AWS Ubuntu Instance

- Instance A: Mysql Server
- Instance B: Mysql Server

On mysql client Linux Server install MySQL Client software.

         sudo apt install mysql-server

By default, both of my EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so i will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, I will not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of my ‘mysql client’.

![ec2-servers](https://user-images.githubusercontent.com/101070055/232151014-3aac88ae-b66d-4ebf-a804-ae17923504fc.png)

SSh into Mysql Server to configure allow connections from remote hosts

        sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
        
Scroll down to mysqld Settings, change bind address to 0.0.0.0, save and exit.

![bind-address](https://user-images.githubusercontent.com/101070055/232155263-13d514a4-7c3c-4351-be40-77fb33e809b9.png)

save and exit

Access the mysql with default login

          mysql

Set root login password "Jmcglobal" >> as password

        ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Jmcglobal';

Create User to connect from anywhere not localhost

        CREATE USER 'username'@'%' IDENTIFIED BY '<password';
        
Grant all database privileges to the user

          GRANT ALL ON *.* TO 'username'@'%';
          
Reload privileges from the grant tables in the mysql database.

        FLUSH PRIVILEGES;
        
 ![table-ok](https://user-images.githubusercontent.com/101070055/232166356-8a7d2c03-afde-429f-8a25-6fa1fbfdbb05.png)

Reference Command to Create table, List table, insert items, delete items, delete table, create database, delete database.

