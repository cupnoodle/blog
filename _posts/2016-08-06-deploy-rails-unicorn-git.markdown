---
layout: post
title:  "Deploy Rails using Unicorn and Git (using DigitalOcean one-click apps)"
date:   2016-08-05 11:10:10
categories: post
post_id: 24
---

Amendment to a [tutorial]({% post_url 2016-06-04-deploy-rails-passenger-rubyconf %}) I wrote for an upcoming workshop in my city. This tutorial will use [DigitalOcean](https://m.do.co/c/f7f1b47b1fff) and its one-click install Ruby + Rails + Nginx + Unicorn image as base. Sign up using [this link](https://m.do.co/c/f7f1b47b1fff) for $10 free credit!  This tutorial is aimed for Ruby on Rails developer who can't afford a PaaS like Heroku  💸, or want to learn how to setup rails on their own VPS.

Disclaimer : I am no DevOps expert and the steps outlined in this tutorial might not be the best way to deploy.

## Prerequisite
You will need a DigitalOcean account created as we will be using its one-click install apps for this tutorial.

## Create the One-click Rails App droplet 

First, log in to [DigitalOcean Control Panel](https://cloud.digitalocean.com/droplets).

Select **Create Droplet**  
![create_droplet](https://littlefoximage.s3.amazonaws.com/post24/create-droplet.png)  

Select **One-click Apps**
![one-click-apps](https://littlefoximage.s3.amazonaws.com/post24/one-click-apps.png)

Select **Ruby on Rails on 14.04 image**  
As for the server size, 512 MB would suffice for this tutorial.
![rails](https://littlefoximage.s3.amazonaws.com/post24/rails-512.png)  

Next, select any additional settings such as private networking, IPv6, or backups.

Finally, select applicable SSH keys if you want to use for accessing this Droplet, and click the Create Droplet button.

## Access your Rails droplet

You can navigate to your newly created droplet ip address using your favorite web browser by typing <code>http://<span style="color:red;">server.ip.address.here</span></code>, you will see rails is up and running.  

![default rails page](https://littlefoximage.s3.amazonaws.com/post24/default-rails.png)

Once the droplet is created, two user accounts **root** and **rails** will be created. Lets login as root via ssh using the following command : 
<pre>
  ssh root@<span style="color:red;">server.ip.address.here</span>
</pre>

Replace <strong>server.ip.address.here</strong> with your server IP address. You will be prompted to input password that is sent to you by email if you didn’t specify ssh key during server creation. If it is your first time connecting to ther server, you will be prompted about host authenticity, choose allow/yes.  

After login, you will be shown the login credentials of the **rails** user and also the PostgreSQL credential like this :  
// insert screenshot here  
Copy this information to a text file as we will use it later.

Since <strong>root</strong> has all the privilleges, it might be dangerous to execute commands as root has the absolute power to do anything and might damage the system if not careful. Lets add sudo privilege to the <strong>rails</strong> user and also add it to the rvm user group so that multiple ruby versions will be available to the user. 

<code>gpasswd -a <strong>rails</strong> sudo</code><br>
<code>gpasswd -a <strong>rails</strong> rvm</code><br>

