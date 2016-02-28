---
layout: post
title:  "How to install and deploy Rails on Nginx without Capistrano or Unicorn"
date:   2015-05-19 20:55:10
categories: post 
---

<h2><span style="font-weight: normal;">Introduction</span></h2>
If you arrived here from google, I am sure you have googled “How to deploy rails” many times and most of the result includes Capistrano or Unicorn. I scratched my head a lot of times wondering what the heck is capistrano and unicorn, I know they can lead me to being more productive but I just want to get the rails app running on my server without much hassle. <br><br>
Another result I got is that the tutorial tell me to just run the following command on the server <br><br>
{% highlight bash %}
rails new myapp
cd myapp
rails s
{% endhighlight %}
<br>
Wtf? I want to know how to transfer my rails file to the server and run them, not directly creating the new rails app on the server and then I have to modify the file directly on the server. On a side note, the <i>rails s</i> command is not suitable for production use, it should only be used for local development and testing purpose only.
Following is the step to install Nginx, ruby and rails on a Ubuntu server. For other linux distros, replace <i>apt-get</i> with their respective command. It is advised to execute the following commands using non-root user.

<h2><span style="font-weight: normal;">Step Zero - Prerequisite and advices</span></h2>
This tutorial assume you have a virtual private server running on ubuntu, a client machine (the local machine you are using now) with rails installed. This tutorial focuses on how to get your local rails app online which some database/assets migration part are omitted which you might need to take note of. Like <code>rake db:migration </code> and <code> rake assets:precompile </code> when deploying online, click <a href="http://edgeguides.rubyonrails.org/active_record_migrations.html"> here for database migration official guide </a> and <a href="http://guides.rubyonrails.org/asset_pipeline.html">here for asset pipeline official guide</a>.  


<h2><span style="font-weight: normal;">Step One - Install RVM</span></h2>
First, be sure to keep the package repository up to date  
{% highlight bash %}
sudo apt-get update
{% endhighlight %}
<br>
Next, install Ruby Version Manager (RVM) to manage multiple version of Ruby language, no worries, we will only install the latest one for this tutorial. To install RVM, type this command :  
{% highlight bash %}
curl -L get.rvm.io | bash -s stable
{% endhighlight %}  
If the GPG signature verification fails, follow the listed command from the returned result, usually in this format :  
{% highlight bash %}
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
{% endhighlight %}  
Then run the install rvm command again.<br>
After finish installing, load RVM.  
{% highlight bash %}
source ~/.rvm/scripts/rvm
{% endhighlight %}  
RVM has some dependencies which are required to work, to install them, type :  
{% highlight bash %}
rvm requirements
{% endhighlight %}  

<h2><span style="font-weight: normal;">Step Two - Install Ruby</span></h2>
Use the rvm command to install ruby, the latest stable version of ruby as of writing this post is 2.2.2, you can replace this version number with the version you like, I will be using version 2.2.0.  
{% highlight bash %}
rvm install 2.2.0
{% endhighlight %}  
Instruct the system to use version 2.2.0 as the default ruby version  
{% highlight bash %}
rvm use 2.2.0 --default
{% endhighlight %}  

<h2><span style="font-weight: normal;">Step Three - Install RubyGems</span></h2>
RubyGems is a package manager for ruby and rails is a gem. Before installing rails, we have to make sure the RubyGems is up to date.  
{% highlight bash %}
rvm rubygems current
{% endhighlight %}  

<h2><span style="font-weight: normal;">Step Four - Install Rails</span></h2>
After installing rubygems, you can now install rails by typing the following command.  
{% highlight bash %}
gem install rails --no-ri --no-rdoc
{% endhighlight %}  
This may take a while depending on your server internet connection, the <i>—no-ri —no-rdoc</i> flag tell the computer not to install the documentation for the gem, documentation take up a large portion of the installation which you might not even refer to in the future. If you want to install the documentation, remove the <i>—no-ri —no-rdoc</i> flag.    
<h2><span style="font-weight: normal;">Step Five - Install Passenger</span></h2>
Passenger is an application server which support ruby, python and node.js. It simplify the deployment process of rails on nginx. Passenger is available as a gem too, simply type :  
{% highlight bash %}
gem install passenger --no-ri --no-rdoc
{% endhighlight %}  
Similar to previous step, if you want the documentation, just remove the <i>—no-ri —no-rdoc</i> flag.<br>
<h2><span style="font-weight: normal;">Step Six - Install Nginx</span></h2>
Now we just need to type the following command to install the Nginx web server module into the passenger.  
{% highlight bash %}
rvmsudo passenger-install-nginx-module
{% endhighlight %}
type in your password if it asks, and you will be presented to a screen like this :
<img src="https://s3-ap-southeast-1.amazonaws.com/littlefoximage/post13/B6153B99-51CE-4CDC-8567-3EA27D2C2C6F.png"><br>
Use space to select the language/framework you want, for this tutorial, I selected only ruby as I only needed ruby for rails to work. Press enter after you have selected.<br><br>
You may stumble across the screen shown below, no worries.
<img src="https://s3-ap-southeast-1.amazonaws.com/littlefoximage/post13/A53776D1-5316-4A41-BEFE-7E8936C7DA7D.png"><br>
Just follow the provided instruction and type :  
{% highlight bash %}
sudo apt-get install libcurl4-openssl-dev
{% endhighlight %}  
After installing the missing dependency, press the up arrow key twice and run the nginx module installation command again.  
{% highlight bash %}
rvmsudo passenger-install-nginx-module
{% endhighlight %}  
You may stumble across this screen if your server has less than 1GB of RAM, no worries, we will follow the instruction given and add the swap space. If your server does not encounter this warning, feel free to skip this step and move on to the next one.
<img src="https://s3-ap-southeast-1.amazonaws.com/littlefoximage/post13/0CC55DF2-CFAF-4496-87D6-90B5286385FB.png">
Press Ctrl-C to abort the installation then type the following command to add swap space :  
{% highlight bash %}
sudo dd if=/dev/zero of=/swap bs=1M count=1024
sudo mkswap /swap
sudo swapon /swap
{% endhighlight %}  
After adding swap, run the installation command again (phew). You will be shown a screen like this :
<img src="https://s3-ap-southeast-1.amazonaws.com/littlefoximage/post13/136B7477-B866-4342-995A-5719DB441B6F.png">
Type 1 and press enter unless you know what you are doing ^^;. Press enter when it ask which directory to install to (default is /opt/nginx) . Grab a bite or coffee meanwhile the installation is running, this might take a while.
<img src="https://s3-ap-southeast-1.amazonaws.com/littlefoximage/post13/C42E4962-D5C6-48F9-9884-708017524E05.png">
After finishing installation, you will see a screen like this :
<img src="https://s3-ap-southeast-1.amazonaws.com/littlefoximage/post13/8E358E80-FEB7-43BD-9730-48610CB3536B.png">
Press enter and continue. Take note of the nginx configuration file location.Now we will add nginx as a service to the system so next time we can start/stop it easily. (Taken from&nbsp;<a href="http://askubuntu.com/questions/257108/trying-to-start-nginx-on-vps-i-get-nginx-unrecognized-service" style="line-height: 1.4;">http://askubuntu.com/questions/257108/trying-to-start-nginx-on-vps-i-get-nginx-unrecognized-service</a>&nbsp;) Copy and paste the following code and run it.   
{% highlight bash %}
# Download nginx startup script
wget -O init-deb.sh https://www.linode.com/docs/assets/660-init-deb.sh
# Move the script to the init.d directory & make executable
sudo mv init-deb.sh /etc/init.d/nginx
sudo chmod +x /etc/init.d/nginx
# Add nginx to the system startup
sudo /usr/sbin/update-rc.d -f nginx defaults
{% endhighlight %}
<br><br>
To turn on the Nginx web server (as it does not auto turn on after installation), type in the command :  
{% highlight bash %}
sudo service nginx start 
{% endhighlight %}  
Now you can access <i>http://your_server_ip_address</i> and you will see a screen like this :
<img src="https://s3-ap-southeast-1.amazonaws.com/littlefoximage/post13/FE2FE13E-66B8-486E-8554-2C0CC006715D.png">
Congratulation! You have successfully installed Nginx web server. Before we move on, install NodeJS first.  
{% highlight bash %}
sudo apt-get install nodejs
{% endhighlight %}

<h2><span style="font-weight: normal;">Step Seven - Deploy Rails via FTP/SFTP</span></h2>
So now you have installed all the necessary stuff in the server, we will create a sample rails app on the local machine (the computer you are using now). We assume you have rails and all the dependencies installed in your computer. You can skip this step if you already developed a rails app in local machine.  
{% highlight bash %}
rails new testapp
cd testapp
{% endhighlight %}  
Generate a new controller called “demo"  
{% highlight bash %}
rails generate controller demo
{% endhighlight %}  
Open up demo_controller.rb in <i>testapp/app/controller </i>and write this into the file:  
{% highlight ruby %}
class DemoController < ApplicationController
  def index
  end
end
{% endhighlight %}  
And then go to <i>testapp/app/views/demo</i> and create a new file named <i>index.html.erb</i>, type in the following or anything you like into the file and save.
{% highlight html %}
<h2>First rails app deployment! Yay! </h2>
{% endhighlight %}  
After that, go to <i>testapp/config</i> and open <i>routes.rb&nbsp;</i>. Edit the file and set the root to <i>demo#index, routes.rb </i>should look like this :
{% highlight ruby %}
Rails.application.routes.draw do
  root 'demo#index'
end
{% endhighlight %}  
Then go terminal and navigate to the <i>testapp</i> directory and run:  
{% highlight bash %}
rails s
{% endhighlight %}  
Open your browser and type in <i>http://locahost:3000 </i>and you see a screen like this :  
<img src="https://s3-ap-southeast-1.amazonaws.com/littlefoximage/post13/E9751612-709A-4FD1-AAB7-CD099674D9D7.png"><br>
Congratulation! You have created a sample rails app, now we will proceed to uploading the rails app to the server. Open your favourite FTP client and connect to your server, my favourite FTP client is Cyberduck. Move to “<i>/home/<u>username</u></i>” where <u><i>username</i></u> is your username on the server.
Upload the whole <i>testapp</i> folder into the directory mentioned above.  <br><br>
<img src="https://s3-ap-southeast-1.amazonaws.com/littlefoximage/post13/DCC142BC-512E-4952-BEAE-98FA0840F724.png">
<br>After uploading, navigate to<i> /opt/nginx/conf</i>&nbsp;and open <i>nginx.conf</i>&nbsp;to edit<i>.&nbsp;</i>
Find this snippet of code around line 46 :  
{% highlight nginx %}
location / {
    root   html;
    index  index.html index.htm;
}

{% endhighlight %}  

Then add the following lines after the code shown above (replace <u><i>username</i></u> with your own username):  
{% highlight nginx %}
location ~ ^/testapp(/.*|$) {
    root /home/username/testapp/public; # <-- be sure to point to 'public'!
    passenger_app_root /home/username/testapp;
    passenger_base_uri /testapp; 
    passenger_enabled on;
    rack_env production;
}

{% endhighlight %}
So it would look like this in the <i>nginx.conf</i> file :
{% highlight nginx %}
location / {
    root   html;
    index  index.html index.htm;
}

location ~ ^/testapp(/.*|$) {
    root /home/username/testapp/public; # <-- be sure to point to 'public'!
    passenger_app_root /home/username/testapp;
    passenger_base_uri /testapp;
    passenger_enabled on;
    rack_env production;
}
{% endhighlight %}  
<br>Save it , go to the testapp rails application folder, run bundle install and generate a production secret key.  
{% highlight bash %}
cd testapp
bundle install
rake secret RAILS_ENV=production
{% endhighlight %}
Copy the hash generated and open the <i>nginx.conf</i> (inside <i>/opt/nginx/conf</i>) file again.Find this snippet of code (around line 38):  
{% highlight nginx %}
server {
        listen       80;
        server_name  localhost;
{% endhighlight %}
And add&nbsp;<span style="line-height: 1.4;">this line just below&nbsp;<i>server_name locahost;</i></span>Replace the <i>generated_hash</i> with the hash you got just now.  
{% highlight nginx %}
passenger_env_var SECRET_KEY_BASE generated_hash;
{% endhighlight %}  
So it will become like this:  
{% highlight nginx %}
server {
        listen       80;
        server_name  localhost;
        passenger_env_var SECRET_KEY_BASE generated_hash;
{% endhighlight %}  
Save the <i>nginx.conf</i> file and restart the nginx web server.  
{% highlight bash %}
sudo service nginx restart
{% endhighlight %}  
Now you should able to access the testapp rails application by going to <i>http://your_server_ip_address/testapp</i>If all goes well, you should see the output like this:
<img src="https://s3-ap-southeast-1.amazonaws.com/littlefoximage/post13/8302A0BF-36D2-4E93-8133-BE2FC4E8B7F8.png">
Congratulation! You have successfully deployed a rails app from your local machine to the server.

<h2><span style="font-weight: normal;">Epilogue</span></h2>
Ruby on rails is an agile framework which helps developer to produce a prototype quickly, but the steps of deploying and setting environment variable often go undocumented or poorly documented or too advanced (capistrano/unicorn and stuff) that I can’t find a single guide on google which tell me that FTP is actually one of the way to deploy and how to set environmental variables on Nginx-Passenger. (Editing <i>~/.bashrc</i> does not work for Nginx)
I hope you enjoyed this tutorial, please comment below if you have encountered any problem for this tutorial. I have tested all the steps written above on a virtual server running Ubuntu 12.04.



