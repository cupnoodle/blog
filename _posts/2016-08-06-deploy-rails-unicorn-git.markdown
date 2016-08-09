---
layout: post
title:  "Deploy Rails using Unicorn and Git (using DigitalOcean one-click apps)"
date:   2016-08-05 11:10:10
categories: post
post_id: 24
---

Amendment to a [tutorial]({% post_url 2016-06-04-deploy-rails-passenger-rubyconf %}) I wrote for an upcoming workshop in my city. This tutorial will use [DigitalOcean](https://m.do.co/c/f7f1b47b1fff) and its one-click install Ruby + Rails + Nginx + Unicorn image as base. Sign up using [this link](https://m.do.co/c/f7f1b47b1fff) for $10 free credit!  This tutorial is aimed for Ruby on Rails developer who can't afford a PaaS like Heroku  💸, or want to learn how to setup rails on their own VPS.  
  
In this tutorial we will setup a Ubuntu cloud VPS and push a repository from your local machine to the cloud VPS. We will use [fox_sample](https://github.com/cupnoodle/fox_sample) rails repository for this tutorial.  

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
As for the server size, 1GB memory is recommended but 512 MB would suffice for this tutorial.
![rails](https://littlefoximage.s3.amazonaws.com/post24/rails-512.png)  

Next, select any additional settings such as private networking, IPv6, or backups.

Finally, select applicable SSH keys if you want to use for accessing this Droplet, and click the Create Droplet button.

## Access your Rails droplet and setup

You can navigate to your newly created droplet ip address using your favorite web browser by typing <code>http://<span style="color:red;">server.ip.address.here</span></code>, you will see rails is up and running.  

![default rails page](https://littlefoximage.s3.amazonaws.com/post24/default-rails.png)

Once the droplet is created, two user accounts **root** and **rails** will be created. Lets login as root via ssh using the following command : <br>
<code>ssh root@<span style="color:red;">server.ip.address.here</span></code>

Replace <strong>server.ip.address.here</strong> with your server IP address. You will be prompted to input password that is sent to you by email if you didn’t specify ssh key during server creation. If it is your first time connecting to ther server, you will be prompted about host authenticity, type **yes** and press <kbd>enter</kbd>.  

After login, you will be shown the login credentials of the **rails** user and also the PostgreSQL credential like this :  
![rails credential](https://littlefoximage.s3.amazonaws.com/post24/rails_credential.png)
Copy this information to a text file as we will use it later.

Before anything else, update the apt-get repository <br>
<code>sudo apt-get update</code>
  
### Setup rails user privilege
Since <strong>root</strong> has all the privilleges, it might be dangerous to execute commands as root has the absolute power to do anything and might damage the system if not careful. Lets add sudo privilege to the <strong>rails</strong> user and also add it to the rvm user group so that multiple ruby versions will be available to the user. 

<code>gpasswd -a <strong>rails</strong> sudo</code><br>
<code>gpasswd -a <strong>rails</strong> rvm</code><br>

We will also exempt some commands that normally requires sudo password input for the **rails** user, these commands will be used for the git post-receive hook later on.  

Change current directory to sudoers.d : <br>
<code>cd /etc/sudoers.d/</code><br>

and create a file named **rails** to correspond to the rails user.<br>
<code>nano rails</code><br>

type in the following commands : <br>

<pre>rails ALL=(ALL) NOPASSWD: /usr/sbin/service nginx start,/usr/sbin/service nginx stop,/usr/sbin/service nginx restart,/usr/sbin/service unicorn start,/usr/sbin/service unicorn stop,/usr/sbin/service unicorn restart </pre>

Press <kbd>Ctrl</kbd> + <kbd>X</kbd> to quit and enter 'Y' to save. This will grant rails user permission to start/stop/restart nginx and unicorn server without typing password.  

Type in the following chmod permission just in case, <br>
<code>chmod 0440 /etc/sudoers.d/rails</code>

### Increase swap size for gem installation (Optional)
If you have selected 512 MB RAM previously, it is advised to increase the swap size as you might run out of memory for installation of some gem. You can skip this step if your server has 1GB RAM or more.  
<code>sudo dd if=/dev/zero of=/swap bs=1M count=1024</code><br>
<code>sudo mkswap /swap</code><br>
<code>sudo swapon /swap</code><br>

### Logout and login as rails user
Now logout and login as the **rails** user with the credential you saved earlier  
<code>exit</code><br>
<code>ssh rails@<span style="color:red;">server.ip.address.here</span></code>
You can change the password of this user if you want to : <br>
<code>passwd</code>

## Install git and create a repository
Before this step, you should have login as **rails** user.  

Install git using this command : 
<code>sudo apt-get install git</code><br>

After installing Git, we will now create a bare git repository named <strong>fox_sample.git</strong> and a folder to store the source code <strong>fox_sample</strong>, let's put the repository on your user folder for this tutorial ( you can of course change to other location ).  
<code>cd ~rails </code>  
<code>mkdir fox_sample</code>  
<code>mkdir repo && cd repo</code>  
<code>mkdir fox_sample.git && cd fox_sample.git</code>  
<code>git init --bare</code>  

<strong>--bare</strong> indicates that the repository will have no working files / source code, just the version control.

You will have two directory created :
<ol>
<li><strong>/home/rails/fox_sample</strong> - the working tree </li>
<li><strong>/home/rails/repo/fox_sample.git</strong> - the version control repository</li>
</ol>

Next, we will dive into the git hooks to add a work tree directory (where your source code will be stored), more info about [git hooks here](http://githooks.com/). We will use the <strong>post-receive</strong> hook as it is executed after finished receiving git push.  
  
In the repository directory **fox_sample.git**,
Go to the <strong>hooks</strong> folder  
<code>cd hooks</code>  
Create a file named 'post-receive' :  
<code>nano post-receive </code><br><br>
And paste this and save : 
<script src="https://gist.github.com/cupnoodle/0faef2c737ebb7eb988e2a56b4fe1bad.js"></script>
We will configure Unicorn and Nginx config file before pushing a local rails app to this server.


## Configure Unicorn
By default, the Unicorn server is pointing to the default rails app which we saw at the start of this tutorial. We will change the Unicorn configuration file to point to our **fox_sample** directory.<br><br>
Open **/etc/unicorn.conf** and edit it:  
<code>sudo nano /etc/unicorn.conf</code><br><br>
Find the line that mentions **working_directory**, and replace it to point to your application. Replace **rails_project** with **fox_sample**. When you're done, the line should look like this :  
<div class="code-label">/etc/unicorn.conf excerpt</div>  
<pre>
  working_directory "/home/rails/fox_sample"
</pre><br>
Next, open and edit **/etc/default/unicorn**  
<code>sudo nano /etc/default/unicorn</code><br>

Find the line that mentions APP_ROOT, and replace it to point to your application. Replace **rails_project** with **fox_sample**. When you're done, the line should look like this:
<div class="code-label">/etc/default/unicorn excerpt</div>  
<pre>
  APP_ROOT=/home/rails/fox_sample
</pre><br>
Save it and restart Unicorn by typing : <br>
<code>sudo service unicorn restart</code><br><br>
Next, we will configure the Nginx config files.
  
## Configure Nginx

Nginx which is used as a reverse proxy to Unicorn, needs to know the path of your application's **public** directory. Open and edit the Nginx configuration file:  
<code>sudo nano /etc/nginx/sites-enabled/rails</code><br><br>
Find the line that mention **root**, and change it to point to the **fox_sample** app's public directory. The line should look like this after editing:  
<div class="code-label">/etc/nginx/sites-enabled/rails excerpt</div>  
<pre>
  root /home/rails/fox_sample/public;
</pre><br>
Save and exit.   

Now restart Nginx to put the change in effect : <br>
<code>sudo service nginx restart</code><br><br>

## Push local git repository to server and deploy


