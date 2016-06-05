---
layout: post
title:  "Setup and Deploy Rails using Passenger on VPS - Part 2"
date:   2016-06-04 21:10:10
categories: post
post_id: 22
---

## Recap

This post is a continuation from [here]({% post_url 2016-06-04-deploy-rails-passenger-rubyconf %}). Previously we have installed Ruby, Rails, Nginx, passenger and PostgreSQL. On this post we will install git, configure nginx config file to deploy rails app and git push the rails app from your development machine to the VPS. The step number will continue from the previous one, which is eleven.  

## Step Eleven - Install git and creating a repository

Easily install git on the VPS using this command :  
<code>sudo apt-get install git-core</code><br>

After installing Git, we will now create a blank git repository named <strong>awesomeapp</strong>, let's put the repository on your user folder for this tutorial ( you can of course change to other location ).  
<code>cd ~ </code>  
<code>mkdir awesomeapp</code>  
<code>cd awesomeapp</code>  
<code>git init</code>  

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
<code>rails new awesomeapp -d postgresql</code> 
