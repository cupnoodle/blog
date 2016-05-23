---
layout: post
title:  "Deploy Rails using Passenger on VPS"
date:   2016-05-20 21:10:10
categories: post
post_id: 21
---

A post originally written for an upcoming workshop. This post assumes you have some knowledge on Ruby on Rails but have never touched a server before and have no experience in SysAdmin/DevOps.

## Introduction

If you are a Ruby on Rails developer and can't afford a PaaS like Heroku  💸 or want to learn how to setup rails on your own server, then this tutorial is perfect for you. 
We will be using [Phusion Passenger](https://www.phusionpassenger.com/) as the rails app server and [Nginx](https://www.nginx.com) as the web server. Passenger is one of the easiest app server to install ,configure and have decent performance. For this tutorial, we will install Passenger with Nginx on Ubuntu 14.04 . We will be using [DigitalOcean](https://m.do.co/c/f7f1b47b1fff) for its VPS, you can sign up using [this link](https://m.do.co/c/f7f1b47b1fff) for $10 free credit!

## Step One - Create your server / droplet
  
Create a new Ubuntu 14.04 server, for starter applications, 512MB memory should suffice. You can upgrade it easily should you need more memory in the future. Choose the 32 bit version of Ubuntu if your server memory is less than 4GB, as 64-bit programs consume 50% more memory compared to 32 bit programs. If there is possibility which your server will need more than 4GB RAM in the future, choose the 64 bit version.
![Choose OS](https://littlefoximage.s3.amazonaws.com/post21/choose_os.png)
![Choose Spec](https://littlefoximage.s3.amazonaws.com/post21/choose_spec.png)

## Step Two - Create a Sudo user
  
First you have to login to your server as root, open up terminal and type <br> <code>ssh root@SERVER_IP_ADDRESS</code> <br> where SERVER_IP_ADDRESS is your server IP address. You will be prompted to input password that is sent to you by email if you didn't specify ssh key during server creation. If it is your first time connecting to ther server, you will be prompted about host authenticity, choose allow/yes. You might be prompted to change the root password in this case too.  

Now we will create a user with sudo privilege and we will login as the user after this step. It is dangerous running command as root because root has the absolute power to do anything and you might damage the system using root if not careful.

To create user, replace the _demo_ with your desired name : <br>
<code>adduser demo</code><br><br>
After entering password, you can just press enter to skip the field.  
To grant administrative power (able to execute command with sudo put in front) to the user, type the command below using root account, replace the _demo_ with your username : <br>
<code>gpasswd -a demo sudo</code><br>
The command above will add the user to _sudo_ group.  
Now you can logout and login as this user.  
<code>exit</code>  
<code>ssh demo@SERVER_IP_ADDRESS</code>  
  


