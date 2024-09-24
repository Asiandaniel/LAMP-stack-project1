MY LAMP STACK IMPLEMENTATION IN AWS

The LAMP stack is a widely used software suite in web development, comprising:

- Linux (Operating System)
- Apache (Web Server)
- MySQL (Database)
- PHP (Programming Language)

In the context of AWS, implementing a LAMP stack involves deploying these components on an EC2 instance (a virtual server) to host and manage web applications or websites.

This project highlights the process of setting up a LAMP (Linux, Apache, MySQL, PHP) stack on Amazon Web Services (AWS). I utilized an EC2 instance on AWS and accessed it through PuTTY for SSH configuration.

Step 1: Launching an EC2 Instance
From the AWS console, I started an EC2 instance with Ubuntu Server. I set up the security group with:

- SSH (Port 22) for secure remote access.
- HTTP (Port 80) for web traffic.
- HTTPS  (port 443) 

![image](https://github.com/user-attachments/assets/8e14951e-f101-4bdd-a054-577e3768cf85)
2. Make sure to generate an SSH key pair when launching your instance. This key pair is essential for accessing the instance through port 22.

3. Additionally, when setting up the EC2 instance, it's important to configure a security group with the following inbound rules:

- Allow HTTP traffic on port 80 from any IP address on the internet.
- Allow HTTPS traffic on port 443 from any IP address on the internet.
- Allow SSH traffic on port 22 from any IP address (this rule is enabled by default).

4. Ensure that you select the correct VPC when creating the EC2 instance (in my case, I used the default VPC).
5. # Donwload and connect your instance 

6. Download the private SSH key, take note of its download location, and use it to connect to the EC2 instance from your terminal (assuming you're using a Windows OS) by running the following command:
` ssh -i "your-ec2-key.pem" <username>@IPAddress `
in my scenario, the username is set to `ubuntu`, and the public IP address is `-54-161-21-48`. Please note that this IP address changes each time the instance is stopped and started, unless an Elastic IP is assigned.

![image](https://github.com/user-attachments/assets/01173605-9c6a-4b1d-a045-cbfee948df5b)
# Step 1 - Install Apache and Configure the Firewall
1. First, I need to update and upgrade the package manager's repository.

To do this, after connecting to my instance, I will run the following command in my terminal.
`sudo apt update
sudo apt upgrade -y
`

![image](https://github.com/user-attachments/assets/b284c8d6-62a5-48c8-8133-0df68debb6b3)

 # Install Apache2
3. ` sudo apt install apache2 -y `
![image](https://github.com/user-attachments/assets/b41b237b-4623-466f-9029-09517f63d426)

3. Enable Apache2 and Verify that it is running

   `
   sudo systemctl enable apache2
   sudo systemctl status apache2
`
After running the `systemctl status` command, if I see any green indicators (like in the image below), I can be confident that the service is up and running.

![image](https://github.com/user-attachments/assets/b7bd6565-dc29-495b-9f49-ab5db0173264)

4. I need to verify that the server is running and accessible locally through my Ubuntu shell by running the following command.

` curl http://localhost:80
OR
curl http://127.0.0.1:80
`
5. I will test whether I can access the default Apache page on my server by entering my EC2 public IP into a browser.

![image](https://github.com/user-attachments/assets/1f9da6f3-1cde-434a-9d78-4d92fcbf743f)

` http://54.161.21.48/ `![image](https://github.com/user-attachments/assets/78e4d276-fbf7-4b68-8917-297834a3ace1)

This indicates that the web server has been correctly installed and is now accessible through th firewall.

Step 2 - Install MySQL
MySQL is essential in a LAMP stack for handling all database-related tasks, making your web application functional, dynamic, and data-driven.
` sudo apt install mysql-server `
![image](https://github.com/user-attachments/assets/2f7e0b65-d272-4e7f-b976-5d5b2707c896)

2. I enabled mysql and Ensure that it is running by running these commands:
3.   `sudo systemctl enable --now mysql`
4.   `sudo systemctl status mysql `
5.   ![image](https://github.com/user-attachments/assets/64d968f1-5362-432f-9cef-2c4d60eaaa12)
3. Log in to mysql console
4. `sudo mysql `
5. Running this command will conect my shell to the MySQL server as the admin database user root assumed by the useof 'sudo' when i was executing the sudo mysql command.
6. ![image](https://github.com/user-attachments/assets/64d6cb60-51bd-47a8-b4b0-b08ebacff583)

7. i  Assigned a password to the root user using the mysql_native_password as the default authetication method. The password i assigned to the user for the purpose of this documentation is "DanielSage"
8. ![Uploading image.png…]()

9. ![Uploading image.png…]()

