---
layout: post
title:  "How to install Let's Encrypt SSL/TLS certificate in Nginx"
date:   2016-02-28 22:10:10
categories: post
post_id: 17
emoji: 1
---


##What is HTTPS ?

HTTPS (also known as HTTP Secure) is a secure protocol for the internet. Normal communication in HTTP uses plain-text to transfer data between server and client hence any potential hacker can intercept your connection and retrieve the data in plain text form. With HTTPS, the data transferred between the server and the client is encrypted. HTTPS also verifies the **authenticity** of the website using a SSL/TLS certificate. HTTPS was first used on online banking site and subsequently used by most website to protect user privacy.

**TL;DR** : With HTTPS you can read the Internet, With HTTP the Internet can read you.

## What is Let's Encrypt ?

Obtaining a SSL/TLS certification used to [cost a moderate](https://www.geotrust.com/ssl/) amount of money and then Let's Encrypt appeared.  Generally, SSL/TLS certification is issued by a **Certificate Authority**
