---
layout: post
title:  "Journey of My Side Project - uPush"
date:   2016-06-22 21:10:10
categories: post
post_id: 23
---


<img class='centered' src ="https://littlefoximage.s3.amazonaws.com/post23/uPushLogo.png" alt="uPush Logo" /> 


## How It Started
During my final year in university, an external digital agency collaborated with my university and organized a mobile app competition. I participated the competition with two of my friend and started building the iOS app and backend server together. The inspiration of the app came from the frustration and annoyance I faced with my university online learning portal, which was a customized [Moodle](https://moodle.org/) system.  

## Problems
There are few problems with the customized Moodle system, first being not mobile responsive, it's a minor problem but I usually don't bring laptop to university, downloading lecture note from the portal using mobile is a pain which I have to zoom in every time. The second problem is that some lecturer often post class cancelation notice on the portal at last minute, which we will only know after we check the portal usually after arriving campus. I got really irritated with this problem and wished that I can receive a notification whenever a new notice is posted by the lecturer.

## Implementation

### Mobile app
I was interested with iOS programming that time and took this opportunity to learn Objective-C and basics of iOS programming (delegate, NSURLConnection, viewDidAppear, Grand Central Dispatch etc). I wanted to build an app that can download lecture note and view announcement/notice from the portal, with notification feature whenever a lecturer post a new announcement/notice. Here are some screenshot of the completed app : <br/><br/>
<img class="screenshot" src="https://littlefoximage.s3.amazonaws.com/post23/uPush1.jpg" alt="uPush screenshot" /> 
<img class="screenshot" src="https://littlefoximage.s3.amazonaws.com/post23/uPush2.jpg" alt="uPush screenshot" />
<img class="screenshot" src="https://littlefoximage.s3.amazonaws.com/post23/uPush3.jpg" alt="uPush screenshot" />
<img class="screenshot" src="https://littlefoximage.s3.amazonaws.com/post23/uPush4.jpg" alt="uPush screenshot" />

### Backend
To retrieve the data from the web portal, I coded a web crawler to log into the portal and get data from it. I used ruby [Mechanize](https://github.com/sparklemotion/mechanize) for the web crawling. The portal didn't have any anti web crawler measure that time so any POST request to its endpoint will be treated as a normal login request. I also coded an API using [Rails Metal Controller](http://api.rubyonrails.org/classes/ActionController/Metal.html), a metal controller is used to reduce the resource loaded by rails.
{% comment %}
{% endcomment %}