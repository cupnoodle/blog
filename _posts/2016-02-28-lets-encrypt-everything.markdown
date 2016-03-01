---
layout: post
title:  "How to install Let's Encrypt SSL/TLS certificate on Nginx"
date:   2016-02-28 22:10:10
categories: post
post_id: 19
emoji: 1
---


## What is HTTPS ?

HTTPS (also known as HTTP Secure) is a secure protocol for the internet. Normal communication in HTTP uses plain-text to transfer data between server and client hence any potential hacker can intercept your connection and retrieve the data in plain text form. With HTTPS, the data transferred between the server and the client is encrypted. HTTPS also verifies the **authenticity** of the website using a SSL/TLS certificate. HTTPS was first used for online banking site and subsequently used by most website to protect user privacy.

**TL;DR** : With HTTPS you can read the Internet, With HTTP the Internet can read you.

## What is Let's Encrypt ?

Generally, SSL/TLS certificate is issued by a **Certificate Authority (CA)** and web browsers have a predefined list of trusted CA that they will accept. You can of course self sign a SSL/TLS certificate without going through CA but then the browser will show a scary warning to your site visitor like this :  

![Big fat yellow warning](https://littlefoximage.s3.amazonaws.com/post/19/ssl-warning.png)  
Obtaining a SSL/TLS certificate used to [cost some money](https://www.geotrust.com/ssl/) and then good guy Let's Encrypt appeared.   Let's Encrypt is a CA which provides free SSL/TLS certificate and they have automated the process which you can install it on your own server. 

## Prerequisites

You should have a linux server with user with **sudo** privillege.
This post will be using Ubuntu server for example, you can replace the ubuntu specific command like <code>apt-get</code> with the command of linux distro you use.  

The port 443 is not blocked by the firewall of your server as this port is used by SSL.  

You must own or have control to the domain name that you wish to use with the certificate generated. If you do not have a domain name yet, I recommend [purchasing one at Namecheap](https://www.namecheap.com/?aff=70386).

As Let's Encrypt don't allow you generate certificate for domain name you don't own, you have to prove to them that you own the domain name. Go to DNS Setting of your domain name and create an A Record that points your domain to the public IP address of your server. For example, if you want the certificate to work for both **example.com** and **www.example.com** , you have to add two A records to your server as shown below :   

![Adding both @ and www A Record](https://littlefoximage.s3.amazonaws.com/post/19/a_record.png) 
  

## Step One - Install Let's Encrypt
To obtain certificate from Let's Encrypt, you have to install the **letsencrypt** client software on your server. We will clone it from their GitHub repository to our server.

### Optional - Install Git and BC
You will need to install git and bc in order to clone the repository, skip this step if you have already installed them previously.   
Update the package manager first,  
<code>sudo apt-get update</code>   
Then install git and bc  
<code>sudo apt-get install git bc</code>  

### Clone Let's Encrypt
Now clone the Let's Encrypt repository into **/opt**  
<code>sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt</code>  

## Step Two - Obtain a certificate

To obtain the SSL certificate, we will use the Let's Encrypt standalone plugin.

### Make sure port 80 is not in use
The standalone will run a small web server on **port 80** temporarily to let Let's Encrypt connect to your server and validate your server identity and domain name before issueing you a certificate. (So that you can't pretend to be bankofamerica.com)
You have to stop any process using port 80 before proceeding, if your nginx web server runs on port 80, you have to stop it first :  
<code>sudo service nginx stop</code>  
Run the command below to check if port 80 is in use, if the output is blank means the port is not in use and you can proceed to use the standalone plugin.  
<code>netstat -na | grep ':80.*LISTEN'</code>  

### Run Let's Encrypt standalone plugin
Change to the Let's Encrypt directory  
<code>cd /opt/letsencrypt</code>  
Run the standalone plugin   
<code>sudo ./letsencrypt-auto certonly --standalone</code>  
A prompt will appear like below, enter your email address for notices and key recovery.
![Your email here](https://littlefoximage.s3.amazonaws.com/post/19/your_email.png)  
I have read and agree, sureeeee  
![Ain't nobody got time for that](https://littlefoximage.s3.amazonaws.com/post/19/i-have-read-and-agree.png)  
Enter all the domain names which the certificate will effective for, one certificate can work with multiple domains (eg : __bankofamerica.com__ and __www.bankofamerica.com__)  
![Seems legit](https://littlefoximage.s3.amazonaws.com/post/19/not_your_domain.png)  

If everything goes well, you should see output similar like below :   
<div class="terminal">Output:
IMPORTANT NOTES:
 - If you lose your account credentials, you can recover through
   e-mails sent to you@example.com
 - Congratulations! Your certificate and chain have been saved at
   <b>/etc/letsencrypt/live/bankofamerica.com/fullchain.pem</b>. Your
   cert will expire on <b>2017-02-28</b>. To obtain a new version of the
   certificate in the future, simply run Let's Encrypt again.
 - Your account credentials have been saved in your Let's Encrypt
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Let's
   Encrypt so making regular backups of this folder is ideal.
 </div>  

Take note of the path and expire date of your certificate as highlighted above.

### Where's the cert?


