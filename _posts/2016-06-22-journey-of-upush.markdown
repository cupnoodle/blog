---
layout: post
title:  "Journey of My Side Project - uPush"
date:   2016-06-22 21:10:10
categories: post
post_id: 23
---


<img class='centered' src ="https://littlefoximage.s3.amazonaws.com/post23/uPushLogo.png" alt="uPush Logo" /> 


## How It Started
During my final year in university (2015), an external digital agency collaborated with my university and organized a mobile app competition. I participated the competition with two of my friend and started building the iOS app and backend server together. The inspiration of the app came from the frustration and annoyance I faced with my university online learning portal, which was a customized [Moodle](https://moodle.org/) system.  

## Problems
There are few problems with the customized Moodle system, first being not mobile responsive, it's a minor problem but student usually don't bring laptop to university as most of us walk/cycle to campus and laptop is heavy. Downloading lecture note from the portal using mobile is a pain which I have to zoom in every time. The second problem is that some lecturer often post class cancelation notice on the portal at last minute, which we will only know after we check the portal usually after arriving campus. I got really irritated with this problem and wished that I can receive a notification whenever a new notice is posted by the lecturer.

## Implementation

Feel free to skip this part if you are not into technical stuff 😅.  We named the app **uPush** as a short form for **u**niversity **Push**, as the app will notify you for the latest notice/lecture note that lecturer note has posted to the university portal.

### Mobile app
I was interested with iOS programming that time and took this opportunity to learn Objective-C and basics of iOS programming (delegate, NSURLConnection, Push Notification, Grand Central Dispatch etc). I wanted to build an app that can download lecture note and view announcement/notice from the portal, with notification feature whenever a lecturer post a new announcement/notice. Here are some screenshot of the completed app : <br/><br/>
<img class="screenshot" src="https://littlefoximage.s3.amazonaws.com/post23/uPush1.jpg" alt="uPush screenshot" /> 
<img class="screenshot" src="https://littlefoximage.s3.amazonaws.com/post23/uPush2.jpg" alt="uPush screenshot" />
<img class="screenshot" src="https://littlefoximage.s3.amazonaws.com/post23/uPush3.jpg" alt="uPush screenshot" />
<img class="screenshot" src="https://littlefoximage.s3.amazonaws.com/post23/uPush4.jpg" alt="uPush screenshot" />

### Backend
To retrieve the data from the web portal, I coded a web crawler to log into the portal and get data from it. I used ruby [Mechanize](https://github.com/sparklemotion/mechanize) for the web crawling. The portal didn't have any anti web crawler measure at that time so any POST request to its endpoint will be treated as a normal login request. I also coded an API using [Rails Metal Controller](http://api.rubyonrails.org/classes/ActionController/Metal.html) to let the mobile app retrieve data from the server, a metal controller is used to reduce the resource loaded by rails. 

## The Competition
We were given around 3 months to code the mobile app. After submission, we need to present the app to the judges which comprises two staff from the digital agency and one lecturer from our university. We were pretty confident that we are gonna win the first prize as we coded both the backend and mobile app and we have quite some beta user whereas other teams only developed mockup/prototype of the mobile app with simulated data. We presented the app functionality, show the judge the number of beta users and positive reviews we received from the users. But in the end we didn't win and got a consolation prize instead, I were pretty shocked and saddened that our app lost to some crowdsource story telling app and retail price comparison app with fake data. The judges valued idea more than implementation and we admit that perhaps our idea wasn't that good enough but our app was already production ready that time.  

The lost blows a hard hit to my confidence towards the app, I stopped the development and shut down the backend server shortly after the competition. But two weeks after the competition, I received an email from the digital agency which they expressed interested on the app and would like to  meet up to discuss further on financing and licensing. Honestly I didn't expect this as we didn't even get second runner up for the competition, although we did come up with some business model during the competition. They offered around RM 5000 ([~ 1250 USD](https://www.google.com/search?q=5000+myr+to+usd) ) to fund the development of the app and they wanted 60% of the profit generated from the app itself, they boasted that it is a very generous offer as other agency might even take 70-80% profit cut. We rejected the offer almost immediately as we felt the agency has either underestimate the time needed or the cost needed for the development. 💸  
I regained some confidence after the meeting with the agency, it validated that there exist people who are willing to throw money 💰 at this app. I spinned up the backend server again and gave out beta access of the iOS app around my peers, I didn't further develop the app at this point.

## Reignition
After few months of the competition,  I have graduated from my university. Then a junior (Joshua) shown me an Android app mockup he did for uPush. He was really interested regarding to the app as it provided convenience to other students but at that time there wasn't an Android version and he was using an Android phone. He is really good in coding and he learn really fast, we agreed to let him do the Android app for uPush. During the development of the Android app, we also added timetable feature which student can check the timetable and schedule a reminder before the class, of course the timetable data is retrieved via scraping the university portal. I am really hyped during this period and I never thought that there were people interested enough to help the development of this app.

After few weeks of passing the Android .apk file around our friends for beta testing, we felt that it was time to release it on the Google Play Store to let all of the student from my university know about it. We promoted the app via word of mouth through facebook and we got 600+ sign up  the day after the release : 
![uPush release second day](https://littlefoximage.s3.amazonaws.com/post23/serveronfire.png)
I didn't expect such overwhelming response and the backend server was logging 90% RAM usage which I can't even SSH into it due to insufficient RAM. I used [DigitalOcean](https://m.do.co/c/f7f1b47b1fff) 1GB RAM droplet for the backend server at that time. The server probably looks like this at that time :   
<figure style="text-align:center;">
<img class="centered" src="https://littlefoximage.s3.amazonaws.com/post23/serveronfire2.jpg" alt="Server on fire" />
<figcaption>Screencap from Silicon Valley</figcaption>
</figure>  
To accomodate the load, I had to temporarily turn off the server at midnight and scaled it to 2GB RAM, praise the flexibility of cloud computing ☁️ . After the initial fever, we were getting 50-100 daily new user and 400-700 daily active users. We were really excited about the app and response it received.

## Rival
Not long after the release of uPush, we started seeing the appearance of [WBLE Edition](https://www.facebook.com/wbleedition/), an app which did the exact functions like us, appeared in facebook feed. The app is also developed by a student in the same university and he started promoting in facebook. This further validated our idea as we saw competitor started to appear, instead of worrying about our competitor, we feel more hyped.  

## Request for collaboration which didn't turn well
After the well received response, I emailed the university about uPush app and asked for collaboration as the app is scraping the university data and around 10% of the university students are using uPush at that time. I thought of asking them to develop API for retrieving the information inside the portal which can lessen the burden of both my scraping server and their portal server. 

Of all the features, they only focused on the part which uPush app asked for student login credential, they were very frantic fearing that I might save these credential for future misuse/abuse. They offered to buy the full rights of the app from us and wanted us to stop the development but we declined as we wanted to continue developing/maintaining it. I assured them that the backend server does not store any credential and all of the communication between server and mobile app is encrypted with HTTPS. I even thought of sharing source code with them but they already started to implement anti-scraping measure on the portal almost instantly after the initial email.

## Fun

{% comment %}
{% endcomment %}