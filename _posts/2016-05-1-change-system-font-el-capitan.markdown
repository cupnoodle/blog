---
layout: post
title:  "Using custom system font for OS X El Capitan"
date:   2016-04-28 22:10:10
categories: post
post_id: 20
emoji: 1
---



OS X El Capitan (10.11) used a new font, San Francisco which is tweaked minorly from Helvetica Neue. I am not a big fan of Helveltica Neue/San Francisco as it looked kinda too thin and almost unreadable on some non high-res monitor. Lucida Grande from Pre-Yosemite era still looked better to me. Below is the step to change the default system fonts.

## Premade app for Lucida Grande font (Easy)

So you want to change the default system font to Lucida Grande? Good guy Schreiberstein has [already made an Automator script app](https://github.com/schreiberstein/lucidagrandeelcapitan) that will replace the system font from San Francisco to Lucida Grande in few mouse clicks. Just download the zip file from the repository and open "Lucida Grande El Capitan.app" and click "Patch & Install & Clear font cache" and logout/restart and you are done! You can [download the zipped app here](https://goo.gl/33KNpm).

## How about other fonts? (Advanced)

But what if Lucida Grande is just not your cup of tea? What if you want to change it to Avenir Next or other fonts? There seems to be no generic premade app for changing font, but there is a [System font patcher](https://github.com/dtinth/YosemiteAndElCapitanSystemFontPatcher) which can patch normal font using [FontForge](https://github.com/fontforge/fontforge) to be usable by system.  
  
I tried the System font patcher script in an Ubuntu VM as some user reported that it doesn't work in Homebrew. Feel free to run the script in your local mac as I will use Ubuntu VM in the following step.