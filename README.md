# Overview

## Fundamentals of Ansible

Introduces the basic use cases of Ansible followed by an introduction to Ansible Inventory, Playbooks, Modules, Variables, Conditionals, Loops and Roles. 
Each section is accompanied by a set of coding case point showing my hands-on experience in development and application usage in an Ansible envirnonment.


This project shows:

- Introduction to Ansible
- Introduction to YAML and Hands-on Exercises
- Build Ansible Playbook Files with Hands-on Exercises
- Build Ansible Inventory Files with Hands-on Exercises
- Automate provisioning and web server deployment

In this project I will create:

- Ansible Inventory
- Ansible Playbooks
- Ansible Modules
- Ansible Variables
- Ansible Conditionals
- Ansible Loops and Roles


# Why Ansible?

Ansible is a powerful tool for automation to the provision of the target environment and to then deploy the application. It helps you with configuration management, application deployment, task automation, and also IT orchestration. It can run tasks in a sequence and create a chain of events happening on different servers or devices. 
 
It's simple, powerful, and agentless providing provisioning, config management, continuous delivery, application deployment, and security compliance!

# Ansible's Features

    Ansible's natural automation language allows sysadmins, developers, and IT managers to complete automation projects in hours, not weeks.
    Ansible uses SSH by default instead of requiring agents everywhere. Avoid extra open ports, improve security, eliminate "managing the management", and reclaim CPU cycles.
    Ansible automates app deployment, configuration management, workflow orchestration, and even cloud provisioning all from one system.

# What will we use?

- Ansible
- Oracle VMware VirtualBox
- YAML
- CentOS
- Linux Administrator Skills

# Section 10: Introduction to E-commerce website project

This is a sample e-commerce application built for learning purposes.

Here's how to deploy it on CentOS systems:
# Deploy Pre-Requisites

    Install FirewallD

sudo yum install -y firewalld
sudo service firewalld start
sudo systemctl enable firewalld

# Deploy and Configure Database

    Install MariaDB

sudo yum install -y mariadb-server
sudo vi /etc/my.cnf
sudo service mariadb start
sudo systemctl enable mariadb

    Configure firewall for Database

sudo firewall-cmd --permanent --zone=public --add-port=3306/tcp
sudo firewall-cmd --reload

    Configure Database

$ mysql
MariaDB > CREATE DATABASE ecomdb;
MariaDB > CREATE USER 'ecomuser'@'localhost' IDENTIFIED BY 'ecompassword';
MariaDB > GRANT ALL PRIVILEGES ON *.* TO 'ecomuser'@'localhost';
MariaDB > FLUSH PRIVILEGES;

    ON a multi-node setup remember to provide the IP address of the web server here: 'ecomuser'@'web-server-ip'

   4.Load Product Inventory Information to database

Create the db-load-script.sql

    cat > db-load-script.sql <<-EOF
    USE ecomdb;
    CREATE TABLE products (id mediumint(8) unsigned NOT NULL auto_increment,Name varchar(255) default NULL,Price varchar(255) default NULL, ImageUrl varchar(255) default NULL,PRIMARY KEY (id)) AUTO_INCREMENT=1;

    INSERT INTO products (Name,Price,ImageUrl) VALUES ("Laptop","100","c-1.png"),("Drone","200","c-2.png"),("VR","300","c-3.png"),("Tablet","50","c-5.png"),("Watch","90","c-6.png"),("Phone Covers","20","c-7.png"),("Phone","80","c-8.png"),("Laptop","150","c-4.png");

    EOF

Run sql script


    mysql < db-load-script.sql

# Deploy and Configure Web

   1. Install required packages

    sudo yum install -y httpd php php-mysql
    sudo firewall-cmd --permanent --zone=public --add-port=80/tcp
    sudo firewall-cmd --reload

   2. Configure httpd

Change DirectoryIndex index.html to DirectoryIndex index.php to make the php page the default page

    sudo sed -i 's/index.html/index.php/g' /etc/httpd/conf/httpd.conf

   3. Start httpd

    sudo service httpd start
    sudo systemctl enable httpd

   4. Download code

    sudo yum install -y git
    git clone https://github.com/kodekloudhub/learning-app-ecommerce.git /var/www/html/

   5. Update index.php

Update index.php file to connect to the right database server. In this case localhost since the database is on the same server.

    sudo sed -i 's/172.20.1.101/localhost/g' /var/www/html/index.php

              <?php
                        $link = mysqli_connect('172.20.1.101', 'ecomuser', 'ecompassword', 'ecomdb');
                        if ($link) {
                        $res = mysqli_query($link, "select * from products;");
                        while ($row = mysqli_fetch_assoc($res)) { ?>

    > ON a multi-node setup remember to provide the IP address of the database server here.

    sudo sed -i 's/172.20.1.101/localhost/g' /var/www/html/index.php

   6. Test

curl http://localhost

# Project Solution
   ## Section 10: Ansible E-Commerce Project
    Ansible Playbook Project Solution - Is a working Ansinble playbook in yaml form that will do all of the above.
    Deploy-Configure-Database 1-2.png - Installing MariaDB
    Deploy-Configure-Database 2-2.png - Checking if MariaDB was installed correctly and is working
    Deploy-Configure-Database 3-1.png - Configure firewall for Database(port 3306)
    Deploy-Configure-Database 3-2.png - Configuring Database and creating ecomdb and ecomuser with all permissions
    Deploy-Configure-Database 4-1.png - Load Product Inventory Information to database and Create the db-load-script.sql. We then run the sql script
    Deploy-Configure-Web 1-1.png - Installing the required packages(php-mysql)
    Deploy-Configure-Web 1-2.png - Configuring the firewall(port 80 http)
    Deploy-Configure-Web 2-1.png - Checking if Apache HTTP Server was installed correctly and is working
    Deploy-Configure-Web 4-1.png - Installing Git and and cloning repositry to appropriate location
    Deploy-Configure-Web 5-1.png - Configure httpd and changing the DirectoryIndex index.html to DirectoryIndex index.php to make the php page the default page
    Deploy-Configure-Web 5-2.png - Checking if data was inserted correctly into the table using the appropriate sql script on MariaDb
    Deploy-Pre-Requisites 1-1.png - Installing FirewallD
    Deploy-Pre-Requisites 1-2.png - Start and enable Firewalld to automatically start when the system reboots
    Deploy-Pre-Requisites 1-3.png - Checking if firewall for database was installed correctly and is working
    
## Big thank you to Mumshad Mannambeth and KloudKode for this project and course. Be sure to visit their repository below for further information. Credits below.
[1] KloudKode (2022) learning-app-ecommerce [https://github.com/kodekloudhub/learning-app-ecommerce#introduction].
