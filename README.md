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