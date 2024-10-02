# MY LAMP STACK IMPLEMENTATION IN AWS

The LAMP stack is a widely used software suite in web development, comprising:

- Linux (Operating System)
- Apache (Web Server)
- MySQL (Database)
- PHP (Programming Language)

In the context of AWS, implementing a LAMP stack involves deploying these components on an EC2 instance (a virtual server) to host and manage web applications or websites.

This project highlights the process of setting up a LAMP (Linux, Apache, MySQL, PHP) stack on Amazon Web Services (AWS). I utilized an EC2 instance on AWS and accessed it through PuTTY for SSH configuration.

## Launching an EC2 Instance
From the AWS console, I started an EC2 instance with Ubuntu Server. I set up the security group with:

- SSH (Port 22) for secure remote access.
- HTTP (Port 80) for web traffic.
- HTTPS  (port 443) 

![image](https://github.com/user-attachments/assets/8e14951e-f101-4bdd-a054-577e3768cf85)
 Make sure to generate an SSH key pair when launching your instance. This key pair is essential for accessing the instance through port 22.

Additionally, when setting up the EC2 instance, it's important to configure a security group with the following inbound rules:

- Allow HTTP traffic on port 80 from any IP address on the internet.
- Allow HTTPS traffic on port 443 from any IP address on the internet.
- Allow SSH traffic on port 22 from any IP address (this rule is enabled by default).

 Ensure that you select the correct VPC when creating the EC2 instance (in my case, I used the default VPC).
## Donwload and connect your instance 

Download the private SSH key, take note of its download location, and use it to connect to the EC2 instance from your terminal (assuming you're using a Windows OS) by running the following command:
` ssh -i "your-ec2-key.pem" <username>@IPAddress `
in my scenario, the username is set to `ubuntu`, and the public IP address is `-54-161-21-48`. Please note that this IP address changes each time the instance is stopped and started, unless an Elastic IP is assigned.

![image](https://github.com/user-attachments/assets/01173605-9c6a-4b1d-a045-cbfee948df5b)
#  - Install Apache and Configure the Firewall
 First, I need to update and upgrade the package manager's repository.

To do this, after connecting to my instance, I will run the following command in my terminal.
`sudo apt update
sudo apt upgrade -y
`

![image](https://github.com/user-attachments/assets/b284c8d6-62a5-48c8-8133-0df68debb6b3)

 ## Install Apache2
 ` sudo apt install apache2 -y `
![image](https://github.com/user-attachments/assets/b41b237b-4623-466f-9029-09517f63d426)

## Enable Apache2 and Verify that it is running

   `
   sudo systemctl enable apache2
   sudo systemctl status apache2
`
After running the `systemctl status` command, if I see any green indicators (like in the image below), I can be confident that the service is up and running.

![image](https://github.com/user-attachments/assets/b7bd6565-dc29-495b-9f49-ab5db0173264)

I needed to verify that the server is running and accessible locally through my Ubuntu shell by running the following command.

` curl http://localhost:80
OR
curl http://127.0.0.1:80
`
I will test whether I can access the default Apache page on my server by entering my EC2 public IP into a browser.

![image](https://github.com/user-attachments/assets/1f9da6f3-1cde-434a-9d78-4d92fcbf743f)

` http://54.161.21.48/ `![image](https://github.com/user-attachments/assets/78e4d276-fbf7-4b68-8917-297834a3ace1)

This indicates that the web server has been correctly installed and is now accessible through th firewall.

 # Install MySQL
MySQL is essential in a LAMP stack for handling all database-related tasks, making your web application functional, dynamic, and data-driven.
` sudo apt install mysql-server `
![image](https://github.com/user-attachments/assets/2f7e0b65-d272-4e7f-b976-5d5b2707c896)

 I enabled mysql and Ensure that it is running by running these commands:
   `sudo systemctl enable --now mysql`
   `sudo systemctl status mysql `
   ![image](https://github.com/user-attachments/assets/64d968f1-5362-432f-9cef-2c4d60eaaa12)
 Log in to mysql console
 `sudo mysql `
 Running this command will conect my shell to the MySQL server as the admin database user root assumed by the useof 'sudo' when i was executing the sudo mysql command.
 ![image](https://github.com/user-attachments/assets/64d6cb60-51bd-47a8-b4b0-b08ebacff583)

i  Assigned a password to the root user using the mysql_native_password as the default authetication method. The password i assigned to the user for the purpose of this documentation is "DanielSage"
 ![image](https://github.com/user-attachments/assets/d10c625a-26a4-47e8-8099-9cc4a13b53a6)
 
i exited mysql
` exit `

 ## Run the script to secure MySQL

 This security script is included with MySQL by default. It assists in eliminating certain insecure default configurations and restricts access to the database system.
 ` sudo mysql_secure_installation `
 ![image](https://github.com/user-attachments/assets/f3dd4c9f-18b4-4ef8-be9d-3af0b4f7fccb)

#  Install PHP  

 Now, we'll install PHP. So far, we've set up Apache to serve web content and MySQL to manage and store data. Next, we'll install PHP to process code and deliver dynamic content to users.

To configure PHP on our server, we'll need to install the following:

- The PHP package
- `php-mysql` (a PHP module that allows PHP to interact with MySQL databases)
- `libapache2-mod-php` (enables Apache to process and interpret PHP files)

To install all of these components, run the following command.

` sudo apt install php libapache2-mod-php php-mysql `

![image](https://github.com/user-attachments/assets/767dee86-7ffe-401c-a234-b08765075999)

i Confirmed the PHP version by running this command 
`php -v`
![image](https://github.com/user-attachments/assets/fb2a6884-6482-4e98-b263-dd20ee241117)
At this stage, we have fully installed and configured the LAMP stack (Linux, Apache, MySQL, PHP).

To verify the setup, we need to configure an Apache Virtual Host to store the website's files and directories. Virtual Hosts allow multiple websites to be hosted on a single machine, while keeping this hidden from users.

## - Create a virtual host

 _First, I created a document directory for the new website i am about to create near the default web dir (/var/www/html).
  ` sudo mkdir /var/www/my_project_lamp `
  Assign the ownership of the directory to the current user in the session
  `sudo chown -R $USER:$USER /var/www/my_project_lamp `
  ![image](https://github.com/user-attachments/assets/11f25cd3-8be1-4741-b676-fce531945938)
  i created and open a new configuration file in the Apache's site available directory using my preferred command-line editor.  In this case i used vi then i typed the command bellow:
  ` sudo vi /etc/apache2/sites-available/projectlamp.conf`
  This created a new blank file. then i cliked on "i" to enter into the insert mode after which i pasted in the following code. This code serve as a bare-bone configuration
  
  `<VirtualHost *:80>
ServerName projectlamp
ServerAlias www.projectlamp
ServerAdmin Webmaster@localhost
DOcumentRoot var/www/projectlamp
Errorlog   ${APACHE_LOG_DIR}/error.log
CustomLog  ${APACHE_LOG_DIR}/access.log combined
<VirtualHost>`

 ![image](https://github.com/user-attachments/assets/e55dc546-4411-44f1-b410-ed4d7ef4a36c)


  then i use the ls command to show the new file in the sites available directory
  `  sudo ls /etc/apache2/sites-available `
  ![image](https://github.com/user-attachments/assets/edfae555-011b-47ef-8d63-9d16c5afbb29)
  i enabled the new virtual host by by using a2ensite command
  ` sudo a2ensite projectlamp `
  ![image](https://github.com/user-attachments/assets/6b50d4c4-6d9a-4659-a151-459d62955ce5)
  i ran the following command to confirm that my apache  is properly working 
`
 sudo systemctl start apache2
sudo systemctl status apache2
`
  
  ![image](https://github.com/user-attachments/assets/46268f76-3abd-4f86-a547-897a56f94632)

our new website is now active, but the web root/var/www/projectlamp is still empty. so i created an index.html file in that location  so that we can test that the virtual host works as expected  

  `sudo echo 'Hello LAMP from hostname' $(TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
`



Now i went to my browser and try to open my website url usinbg the command below 


` http://<public-ip-Adress>`
![image](https://github.com/user-attachments/assets/dcfc74a0-3f7a-4d83-8cd6-15474160fdc9)



i can leave this file in place as temporary landing page for all my application until i set up an index.php file to replace it, once i do that , i remember to remove or rename the index.html file from my document root, as it would take precedence over an index.php file by default 

# Enable php on the web server 

with the default directoryindex sttings on Apache, a file named index.html will always take precedence over an index.php file. this is useful for setting up maintenance pages in php applications, by creating a temporary index.html file containing an informative message to visitors. Because this page will take precedence over the index.php page, it will then become the landing page for the application. Once maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page   

`sudo vim /etc/apache2/mods-enabled/dir.conf `

![image](https://github.com/user-attachments/assets/b26394a4-6323-4f91-82b5-f37b13c28e55)

after saving and closing and saving the file i rloaded my apache so that the changes will take effect. I make use of this command to reload the Apache 
` sudo systemctl reload apache2 ` 


finally i created a php script to test that php is correctly installed and configured on my server Now that i have a custom location to host my website's files and folders i will create a php test script to confirm that Apache is able to handle and process to php files 

`$ vim /var/www/projectlamp/index.php`
this will open a blank file, then  i added the following text  which is valid PHP code, inside the file 

`<?php
phpinfo();
`
After saving and closing the file, i refreshed the page 

![image](https://github.com/user-attachments/assets/246e559d-e92b-4ec7-b576-1817a8f4a729)


# Conclusion
This project successfully deployed a fully operational LAMP stack on an AWS EC2 instance. The main achievements include:

Setting up a cloud environment on AWS
Installing and configuring Linux, Apache, MySQL, and PHP
Configuring Apache virtual hosts to host multiple websites
The hands-on experience gained through this project has deepened the understanding of cloud computing, server management, and DevOps principles. The acquired skills in scripting, system administration, and cloud infrastructure management are valuable for DevOps and form a strong foundation for more complex projects and deployments.

# Future Enhancements

Implement SSL/TLS for secure connections
Establish a CI/CD pipeline for automated deployments
Explore containerizing the LAMP stack using Docker

#Contributing
Contributions to enhance this project are encouraged. Please fork the repository and submit a pull request with your improvements.


