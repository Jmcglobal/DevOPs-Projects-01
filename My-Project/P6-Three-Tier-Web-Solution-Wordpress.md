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

![serever](https://user-images.githubusercontent.com/101070055/232228808-24e74ff1-5a0a-4091-9412-2feb2b56fef7.png)

Above is my web-server and database EC2 server.

# Next: Volume
 - create volume.
 - Attache Volume.
 - List Volume
 - Partition Volume
 - Mount Volume

I will create 3 volumes of 10GB each on AWS, and attache it to webserver Instace.

![volume](https://user-images.githubusercontent.com/101070055/232230937-4cb363c9-ed56-42d8-aa89-ecf2e169141d.png)

As you can see it is showing available, i will attache the volumes of EC2 web-server

![attach-v](https://user-images.githubusercontent.com/101070055/232231079-d1a2daa8-2d5c-449c-af2f-0e3c6f727765.png)

![volume-attch](https://user-images.githubusercontent.com/101070055/232231089-fa398423-fea3-4cc1-bc88-5b1a51e022e6.png)

This is how i attache the volumes, one after another to the web-server

# Web server volume mount and setup

- Commands 

       df -h >> view all mounted volumes and free space
 
 ![root-volume](https://user-images.githubusercontent.com/101070055/232232465-d96890ac-c373-4739-be47-74f72ed62cab.png)

       lsblk  >> List volumes attached to the server

![availa-volume](https://user-images.githubusercontent.com/101070055/232232527-96b65cef-c694-468a-8bdb-9891ea121e84.png)

 gdisk utility to create a single partition on each of the 3 disks
 
     sudo gdisk /dev/xvdh
 
 PROMPT:
 
       GPT fdisk (gdisk) version 1.0.8

      Partition table scan:
        MBR: not present
        BSD: not present
        APM: not present
        GPT: not present

      Creating new GPT entries in memory.

      Command (? for help): n
      Partition number (1-128, default 1): 
      First sector (34-20971486, default = 2048) or {+-}size{KMGTP}: 
      Last sector (2048-20971486, default = 20971486) or {+-}size{KMGTP}: 
      Current type is 8300 (Linux filesystem)
      Hex code or GUID (L to show codes, Enter = 8300): 
      Changed type of partition to 'Linux filesystem'

      Command (? for help): p
      Disk /dev/xvdh: 20971520 sectors, 10.0 GiB
      Sector size (logical/physical): 512/512 bytes
      Disk identifier (GUID): 4B3E0EF2-E229-431B-8E52-042A491843A3
      Partition table holds up to 128 entries
      Main partition table begins at sector 2 and ends at sector 33
      First usable sector is 34, last usable sector is 20971486
      Partitions will be aligned on 2048-sector boundaries
      Total free space is 2014 sectors (1007.0 KiB)

      Number  Start (sector)    End (sector)  Size       Code  Name
         1            2048        20971486   10.0 GiB    8300  Linux filesystem

      Command (? for help): w

      Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
      PARTITIONS!!

      Do you want to proceed? (Y/N): Y
      OK; writing new GUID partition table (GPT) to /dev/xvdh.
      The operation has completed successfully.

 lsblk utility to view the newly configured partition on each of the 3 disks
 
       lsblk
 
 ![partition](https://user-images.githubusercontent.com/101070055/232233445-227a5ff8-2525-4a87-bbc6-6d8d8b27a0d6.png)

pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM

        sudo pvcreate /dev/xvdf1
        sudo pvcreate /dev/xvdgi
        sudo pvcreate /dev/xvdh1
 
 Confirm if physical volume is created succesfully
 
       sudo pvs

![physical-v](https://user-images.githubusercontent.com/101070055/232234286-46bb2448-849e-4f2b-976d-d9a62bd3149e.png)

vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG app-data

      sudo vgcreate app-data /dev/xvdf1 /dev/xvdg1 /dev/xvdh1

Verify If my VG has been created successfully by running

      sudo vgs

![vg-volume](https://user-images.githubusercontent.com/101070055/232234390-f4a947ef-eeff-4b31-9edc-a160eb89b71f.png)

I will create 2 logical volumes with lvcreate for apps to store website data and while  apps-log will be sued to store data for logs.

      sudo lvcreate -n apps -L 14G app-data
      sudo lvcreate -n apps-log -L 14G app-data

verify if logical volume have been created

      sudo lvs

![lv-volume](https://user-images.githubusercontent.com/101070055/232234840-ceb012de-0c7e-4c12-b56f-6edea9cdadc2.png)

To verify entiire setup

        sudo vgdisplay -v #view complete setup - VG, PV, and LV
        sudo lsblk
        
![lsblk](https://user-images.githubusercontent.com/101070055/232235377-d8d0f215-0490-4151-931b-97eeea2a7ee5.png)
       
Format logical Volume with ext4 filesystem

        sudo mkfs -t ext4 /dev/app-data/apps
        sudo mkfs -t ext4 /dev/app-data/apps-log

Create directory to store website files

       sudo mkdir -p /var/www/html
 
Create home logs directory to store backup of log data

        sudo mkdir /home/backup/logs

Mount /var/www/html on apps logical volume

        sudo mount /dev/app-data/apps /var/www/html/
        
Backup log files in the log directory /var/log into home backup directory /home/backup/logs using "rsync" utility 

      sudo rsync -av /var/log/. /home/backup/logs/

Mount /var/log on apps-log logical volume (caution : all the files on var/log will be deleted, that is why i backed it up )
      sudo mount /dev/app-data/apps-log /var/log/

Restore log files on /home/backup/logs to /var/log with rsync utility

      sudo rsync -av /home/backup/logs/. /var/log/

Update /etc/fstab file so that the mount configuration will persist after restart of the server. 
