# WEB STACK IMPLEMENTATION (LEMP STACK)

Project 2 covers similar concepts as [Project 1](https://github.com/skillembassy20/lamp-stack-implementation-project-1) and helps to cement your skills of deploying Web solutions using LA(E)MP stacks.

You have done a great job with successful completion of Project 1.

In this project you will implement a similar stack, but with an alternative Web Server - [NGINX](https://nginx.org/en/), which is also very popular and widely used by many websites in the Internet.

Side Self Study
1. Make yourself familiar with basic SQL syntax and most commonly used commands
2. Be comfortable using not only VIM, but also Nano editor as well, get to know basic Nano commands

## Step 0 - Preparing prerequisites

In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.

If you do not have an AWS account - go back to **[Project 1 Step 0](https://github.com/skillembassy20/lamp-stack-implementation-project-1)** to sign in to AWS free tier account and create a new EC2 Instance of t2.nano family with Ubuntu Server 20.04 LTS (HVM) image. Remember, you can have multiple EC2 instances, but make sure you **STOP** the ones you are not working with at the moment to save available free hours.

Hint: In previous project we used Putty on Windows to connect to our EC2 Instance, but there is a simpler way that do not require conversion of .pem key to .ppk - using Git Bash.

Launch Git Bash and run following command:
```
ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>
```

It will look like this:

![](./images/image1.png)

## Step 1 – Installing the Nginx Web Server

In order to display web pages to our site visitors, we are going to employ Nginx, a high-performance web server. We’ll use the **`apt`** package manager to install this package.

Since this is our first time using `apt` for this session, start off by updating your server’s package index. Following that, you can use `apt install` to get Nginx installed:

```
$ sudo apt update
$ sudo apt install nginx
```

![](./images/image2.png)

When prompted, enter `Y` to confirm that you want to install Nginx. Once the installation is finished, the Nginx web server will be active and running on your Ubuntu 20.04 server.

To verify that nginx was successfully installed and is running as a service in Ubuntu, run:
```
$ sudo systemctl status nginx
```

![](./images/image3.png)

If it is green and running, then you did everything correctly - you have just launched your first Web Server in the Clouds!

Before we can receive any traffic by our Web Server, we need to open [TCP port 80](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers) which is default port that web brousers use to access web pages in the Internet.

As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to EC2 configuration to open inbound connection through port 80:

![](./images/OpenPort80.gif)

Our server is running and we can access it locally and from the Internet (Source 0.0.0.0/0 means ‘from any IP address’).

First, let us try to check how we can access it locally in our Ubuntu shell, run:
```
$ curl http://localhost:80
or
$ curl http://127.0.0.1:80
```

![](./images/image5.png)

These 2 commands above actually do pretty much the same - they use ***‘curl’*** command to request our Nginx on port 80 (actually you can even try to not specify any port - it will work anyway). The difference is that: in the first case we try to access our server via [DNS name](https://en.wikipedia.org/wiki/Domain_Name_System) and in the second one - by IP address (in this case IP address 127.0.0.1 corresponds to DNS name ‘localhost’ and the process of converting a DNS name to IP address is called “resolution”). We will touch DNS in further lectures and projects.

As an output you can see some strangely formatted test, do not worry, we just made sure that our Nginx web service responds to ‘curl’ command with some payload.

Now it is time for us to test how our Nginx server can respond to requests from the Internet. Open a web browser of your choice and try to access following url

```
http://<Public-IP-Address>:80
```

Another way to retrieve your Public IP address, other than to check it in AWS Web console, is to use following command:
```
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```

The URL in browser shall also work if you do not specify port number since all web browsers use port 80 by default.

If you see following page, then your web server is now correctly installed and accessible through your firewall.

![](./images/Nginx_welcome.png)

In fact, it is the same content that you previously got by ‘curl’ command, but represented in nice HTML formatting by your web browser.

## Step 2 — Installing MySQL

Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.

Again, use apt to acquire and install this software:
```
$ sudo apt install mysql-server
```
![](./images/image6.png)

When prompted, confirm installation by typing `Y`, and then `ENTER`.

When the installation is finished, it’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. Start the interactive script by running:

```
$ sudo mysql_secure_installation
```

![](./images/image7.png)

This will ask if you want to configure the VALIDATE PASSWORD PLUGIN.

```
*Note:** Enabling this feature is something of a judgment call. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.
```

Answer Y for yes, or anything else to continue without enabling.

```
VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No:
```

If you answer “yes”, you’ll be asked to select a level of password validation. Keep in mind that if you enter 2 for the strongest level, you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special characters, or which is based on common dictionary words.

```
There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary              file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1
```

Regardless of whether you chose to set up the`VALIDATE PASSWORD PLUGIN`, your server will next ask you to select and confirm a password for the MySQL **root** user. This is not to be confused with the **system root**. The **database root** user is an administrative user with full privileges over the database system. Even though the default authentication method for the MySQL root user dispenses the use of a password, **even when one is set**, you should define a strong password here as an additional safety measure. We’ll talk about this in a moment.

If you enabled password validation, you’ll be shown the password strength for the root password you just entered and your server will ask if you want to continue with that password. If you are happy with your current password, enter **Y** for “yes” at the prompt:

```
Estimated strength of the password: 100 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
```