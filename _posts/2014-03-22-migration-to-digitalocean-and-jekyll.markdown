---
layout: post
title:  "Migration to DigitalOcean and Jekyll"
date:   2014-03-22 03:05:10
categories: post
---
##Prologue
Konbanwa comrades! I have migrated from previous [shared web hosting][serverfreak] to [DigitalOcean][digitaloceanlink].

I am now using [Jekyll][jekyll] as a blog generator for convenience.

I have also bought the domain [kitsunechan.com][kitsunechancomlink], [kitsunechan.net][kitsunechannetlink] will be still accessible until January 2015 when the hosting will expire. Mind you, the following content is kinda geeky.

##Why the transition to DigitalOcean?
[Server Freak][serverfreak] served me well enough, they have reliable uptime, decent tech support and basic PHP,MySQL, CPanel, Cron jobs ,stuff with affordable price.  
The problem comes in when you want to write ruby apps/rails apps, although they do mention that they support these two, but I had a frustrating experience configuring/installing new ruby apps. They have jekyll ruby gems preinstalled but the installation of jekyll on website using their one-click installer always fail, their shared hosting doesn't allow me to access server with terminal either. The latest version of cpanel (as of today) still can't support Rails 3.0 where as the latest version of Rails is 4.0 (as of today).

![jekyll installation always fail in cpanel, perhaps due to permission problem](https://farm3.staticflickr.com/2841/13361520574_1718536936_o.png)

When I first discovered [DigitalOcean][digitaloceanlink], their pricing really caught me. The cheapest plan which costs $5 per month comes with 512MB memory, 1 CPU Core, 20GB SSD and 1 TB transfer (plus a static IP), who would refuse that? I bought the $5 plan after giving it some thought. The downside of it is that you have to install everything from scratch, they give you only the OS (Ubuntu, Fedora, Debian, etc..), no more one-click Cpanel. They do have some preset OS that comes with Ghost or Rails installed. Everything was done through terminal via ssh. [Stack overflow](http://stackoverflow.com) becomes your best friend when you are configuring linux server. I did messed up some configuration and have to reinstall the OS (ubuntu in this case), the power of SSD shortened the time needed for OS installation from hours to couples of minutes.

##PHP/Apache VS Jekyll/Nginx

[kitsunechan.net][kitsunechannetlink] runs PHP on Apache. The post content is retrieved from MySQL database and parsed dynamically, having a database means there exist possible vulnerability for hacker to attack. Apache is a process-based server , which means that Apache will create a new process for a new request, resulting heavy memory consumption (inclduing request to static page). 
 
I started to experiment with [Jekyll][jekyll] on [kitsunechan.com][kitsunechancomlink], Nginx server is event-based and consume very low memory for serving static page,
 I gave it a thought and decided to make each blog post a static page as there is rarely a change on blog post. Each blog post is written in 
 [Markdown](http://markdowntutorial.com) format and uploaded to server and generated as static HTML by using `jekyll build` command. Less hassle isn't it? 

##Epilogue
Less time spent in worrying technical stuff = more time to focus on blog ^w^/.  

![simple installation of jekyll](https://farm8.staticflickr.com/7360/13362316183_20d4656023_c.jpg)



[jekyll]:    http://jekyllrb.com
[digitaloceanlink]: http://digitalocean.com
[kitsunechancomlink]: http://kitsunechan.com
[kitsunechannetlink]: http://kitsunechan.net
[serverfreak]: http://serverfreak.com