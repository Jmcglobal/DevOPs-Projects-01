In this project i prepare storage infrastructure on two Linux servers and implement a basic web solution using WordPress.
WordPress is a free and open-source content management system written in PHP and paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS).

## Two part of this project.

I will configure storage subsystem for Web and database Servers Based on linux OS
I will install WordPress and connect it to a remote MYSQL database server.

## Project Overview diagram .

![tier](https://user-images.githubusercontent.com/101070055/232207542-a9dceb01-ecc0-413c-a8b5-9f8ce86fb016.jpg)

- Presentation Layer (PL): This is the user interface such as the client server or browser on your laptop.
- Business Layer (BL): This is the backend program that implements business logic. Application or Webserver
- Data Access or Management Layer (DAL): This is the layer for computer data storage and data access. Database Server or File System Server such as FTP server, or NFS Server

# Three-tier requirement

- A Laptop or PC to serve as client.
- An EC2 Linux Server for Web server
- An EC2 Linux Server asa database Server.

NOTE: I am using AWS EC2 Ubuntu instance for this.
