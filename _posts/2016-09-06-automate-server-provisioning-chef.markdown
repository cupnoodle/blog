---
layout: post
title:  "Rails starter pack cookbook (Passenger, Nginx, PostgreSQL, RVM) for Chef and how to use it"
date:   2016-09-06 10:20:10
categories: [devops]
post_id: 26
---

After typing the same installation command over and over again to provision and setup rails server from scratch for personal project/clients, I got tired and thought that there must be a better way to do this. I stumbled upon [Chef](https://www.chef.io/chef) and [Chef-Solo](https://github.com/matschaffer/knife-solo) while Googling on how to automate server provisioning. It got me interested especially it uses Ruby language to define system configuration and you can also insert bash/shell command inside the ruby code.

Before continuing, here are some keywords used in Chef :  

## Chef Terminology  

**Recipe** - A ruby file that contains instruction to install component, usually just one component per recipe.

**Cookbook** - A group of recipes, for example the cookbook used in this post will contain recipe for RVM, Passenger, Nginx and PostgreSQL.

**Node** - A remote server that will be used for provisioning.  

**Role** - A group of recipes that can be applied to a node. Like a 'Webserver role' can contain recipe for Apache and MySQL.  

**Attribute** - Parameter in key value pair in either node or role file used to customize the variable of a recipe. e.g : node[server][domain_name] attribute is used to set the domain name for the server to be provisioned.

## Why I wrote this cookbook
I followed tutorial online and also referenced [Reliably deploying Rails application book](https://leanpub.com/deploying_rails_applications) to learn the basics of Chef. While the book suggest to use battle tested cookbooks from the official [Chef supermarket](https://supermarket.chef.io/), I have a hard time finding a working RVM recipe as the official one from the supermarket is fairly complex and the documentation doesn't specify clearly on how to use it. I also wanted to learn to write a cookbook/recipe from scratch to understand better how Chef works.

## Post disclaimer
This post will just explain enough Chef command to run and provision your server, I suggest reading the [Chef series written by Vladi Gleba](http://vladigleba.com/blog/topics/chef-series/) to understand more on how Chef works.