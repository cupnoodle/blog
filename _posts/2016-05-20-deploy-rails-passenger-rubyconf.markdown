---
layout: post
title:  "Deploy Rails using Passenger on VPS"
date:   2016-05-20 21:10:10
categories: post
post_id: 21
---

A post originally written for an upcoming workshop.

## Introduction

If you are a Ruby on Rails developer and can't afford a PaaS like Heroku  💸 or want to learn how to setup rails on your own server, then this tutorial is perfect for you. 
We will be using [Phusion Passenger](https://www.phusionpassenger.com/) as the rails app server and [Nginx](https://www.nginx.com) as the web server. Passenger is one of the easiest app server to install ,configure and have decent performance. For this tutorial, we will install Passenger with Nginx on Ubuntu 14.04 . We will be using [DigitalOcean](https://m.do.co/c/f7f1b47b1fff) for its VPS, you can sign up using [this link](https://m.do.co/c/f7f1b47b1fff) for $10 free credit!

## Step One - Create your server / droplet
  
Create a new Ubuntu 14.04 server, for starter applications, 512MB memory should suffice. You can upgrade it easily should you need more memory in the future. Choose the 32 bit version of Ubuntu if your server memory is less than 4GB, as 64-bit programs consume 50% more memory compared to 32 bit programs. If there is possibility which your server will need more than 4GB RAM in the future, choose the 64 bit version.
![Choose OS](https://littlefoximage.s3.amazonaws.com/post21/choose_os.png)
![Choose Spec](https://littlefoximage.s3.amazonaws.com/post21/choose_spec.png)

