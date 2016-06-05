---
layout: post
title:  "Setup and Deploy Rails using Passenger on VPS - Part 2"
date:   2016-06-04 21:10:10
categories: post
post_id: 22
---

## Recap

This post is a continuation from [here]({% post_url 2016-06-04-deploy-rails-passenger-rubyconf %}). Previously we have installed Ruby, Rails, Nginx, Passenger and PostgreSQL. On this post we will install git, configure nginx config file to deploy rails app and git push the rails app from your development machine to the VPS. The step number will continue from the previous one, which is eleven.  

## Step Eleven - Install git and creating a repository

Easily install git on the VPS using this command :  
<code>sudo apt-get install git-core</code><br>

After installing Git, we will now create a blank git repository named <strong>awesomeapp</strong>, let's put the repository on your user folder for this tutorial ( you can of course change to other location ).  
<code>cd ~ </code>  
<code>mkdir repo && cd repo</code>  
<code>mkdir awesomeapp.git && cd awesomeapp.git</code>  
<code>git init --bare</code>  

<strong>--bare</strong> indicate that the folder will have no working files, just the version control.  

Type <code>pwd</code> to get your present working directory, you will need this later for adding git remote on your local development machine ( most probably your laptop/pc ). My output looks like this :  
![pwd](https://littlefoximage.s3.amazonaws.com/post22/pwd.png)  

{% comment %}
### Optional but recommended step - Set up git hooks

[Git hooks](http://githooks.com/) are trigger points which you can define commands that will be executed whenever a specific event is triggered. ( eg : restart Nginx server after a git push is received )  
Continuing from previous step, we will edit the post-receive hook for the awesomeapp git repository.  
<code>cd .git</code>

We are inside the inner working of git now, if you type <code>ls</code> to list the directories, you will see output similar to this :  
![Inner mysteries of how git works...or not](https://littlefoximage.s3.amazonaws.com/post22/git_inner.png)  

Now lets go to <strong>hooks</strong> folder

{% endcomment %}

## Step Twelve - Push the Rails app from your local machine to VPS

<strong>Now on your local machine ( your laptop/pc )</strong>, locate to your existing Rails application or create one if you don't have one :  
<code>rails new awesomeapp -d postgresql</code><br>

And then open <strong>/config/database.yml</strong> of the rails app, scroll to bottom and edit the production database username to look like this :  
<script src="https://gist.github.com/cupnoodle/b2b63c556064a89fca1f7c59968316b8.js"></script>  

We will also generate a secret key for the production secret key base.  
<code>cd awesomeapp</code>  
<code>rake secret</code>

![Keep this a secret! Shhh](https://littlefoximage.s3.amazonaws.com/post22/rake_secret.png)  
Copy down the string generated, we will need it later for setting environment variables.  

Remember the present working directory from the previous step? We will add it to the git remote of the Rails app. If your local rails application has not initialize a git repository yet,  
<code>#inside awesomeapp rails root folder </code>  
<code>#cd awesomeapp</code>  
<code>git init</code><br>

Then add the remote : <br>
<code>git remote add <span style="color: #F20B2E;">live</span> ssh://<span style="color: #F20B2E;">user</span>@<span style="color: #F20B2E;">YOUR_SERVER_IP_OR_DOMAIN_NAME</span>/<span style="color: #F20B2E;">repo_location</span>/.git</code>

<strong>live</strong> is the name of the remote, you can change this to whatever name you like.  
<strong>user</strong> is the username you used to login to the ssh, change this to your ssh username.  
<strong>YOUR_SERVER_IP_OR_DOMAIN_NAME</strong> is pretty much self-explanatory.  
<strong>repo_location</strong> is the location of the repository in the VPS, replace this with the present working directory mentioned in previous step.  

Below is the git remote command I used :  
![Git remote add](https://littlefoximage.s3.amazonaws.com/post22/git_add_remote.png)  

And now you are ready to push to the remote VPS, replace <strong>live</strong> with the remote name you have chosen just now.  
<code>git add -A</code>  
<code>git commit -m "Awesome app first commit"</code>  
<code>git push live master</code>  

