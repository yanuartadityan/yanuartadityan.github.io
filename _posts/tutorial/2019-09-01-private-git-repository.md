---
title: 'Private Git Repositories with Gogs on AWS Lightsail'
excerpt: "Thorough step on hosting your own private git repositories using free Gogs web app and host it in AWS"
category: tutorial
layout: post
comments: true
tags: [tutorial, aws, cloud, git]
---

# Introduction

If you are often changing system when you are working on unfinished project, probably you realized that Github's limitation on the private repositories really suck. Because private repository is needed just for saving some state. There is a solution for this but what you need is:

1. An AWS root account
2. Instance of AWS Lightsail (cheapest will do. I'm using $3.5/month Lightsail)
3. A domain for you. I got mine from [Namecheap][s1]
4. Patience :)

# Step 1: Choose Lightsail Service and Build an Instance

Go to AWS home dashboard and choose Lightsail service or go straightaway to [Lightsail][s2] homepage. Then just create an instance by choosing your OS. For me, I picked bare Ubuntu 18.04 instance because I want to install all components manually. Don't forget to choose the closest server to you.

After everything is set, you can access the instance from in-built web based SSH client or use your favorite ssh client. Since I'm running Macos, I just use my favorite iTerm to do this. You can get the default ssh key made for you already by Amazon [here][s3]. Download the key (.pem) and save it in somewhere secure in your computer.

Neat!

# Step 2: Test SSH Connection

To access your instance with SSH, what you need apart from your key is your instance's hostname and public IP. All these information you can get from your instance manage page. If your chosen OS is Ubuntu, then most likely your hostname is *ubuntu*. 

Now, on the SSH part, you can do this command.

`ssh -i [path to the key] [hostname]@[public IP]`

If the ssh prompts you with a confirmation to remember your creds, you can just answer YES and when it is connected, you're already inside your instance bash terminal. 

When it is not, probably you need to ensure that your downloaded key has the right access. You can make it only accessible to you by invoking this simple command:

`chmod 400 [path to the key]`

Redo the ssh command and see if it works.

Since I'll do this ssh command so often, I created an alias in `~/.bash_profile`:

`alias sshlightsail="ssh -i [path to the key] [hostname]@[public IP]`

Then reload your *bash* by doing `source ~/.bash_profile` and next time, just call *sshlightsail*. Voila!

**Note** This will let you as the owner of the instance to connect to your instance and has all root access. It is, in fact, not a good practice. You may want to have multiple people working on your instance, one for this project and one for others. Thus, it is good to create an another user just to work for this tutorial. You can just follow the guide on it [here][s7]. For now, I'm using the default root user provided by the instance.

# Step 3: All the necessary components

Update your instance's Ubuntu by calling `sudo apt-get update`. This will take a while, but usually it's good time to brew your favorite coffee. 

Once you finished your coffee, you might want to know what are the components we need to install. If you have time, it's always good to read about their pages a bit. So, here is the list:

* [Gogs][s4]. A painless self-hosted Git service
* [Nginx][s5]. True magic here
* [Certbot][s6]. To enable secure SSL/HTTPS in your web server

# Step 4: Install Gogs

The first step is to install Gogs. Make sure to get the latest Gogs version from [download page][s8] and make sure you get the AMD64 version of it. For me, I downloaded the .tar.gz version of it.

1. Create a folder for the downloadable `mkdir ~/web/download/`
2. Go into the folder and download `wget https://dl.gogs.io/0.11.91/gogs_0.11.91_linux_amd64.tar.gz`
3. Unpack `tar zxf https://dl.gogs.io/0.11.91/gogs_0.11.91_linux_amd64.tar.gz`
4. Then you'll see **gogs** folder and you can find the Gogs app inside.
5. Try to launch it at port 80 by calling `./gogs web --port=XXXX`

For the step 5, provide your desirable port for the server to listen to. Default HTTP request is 80 and by default Gogs is running at port 3000. Of course, you can redirect incoming request to Gogs to any location later on using Nginx.

Remember your instance public IP and paste it in your browser and VOILA! You'll encounter Gogs' installation page. You don't have finish the installation as we'll configure everything later. At least, now your Gogs app is working fine. 

# Step 5: Configure Gogs

Few things you have to prepare before we continue.

1. Gogs main app configuration file in `/gogs/custom/conf/app.ini`.
2. Gogs *systemd* service file. This is for launching the Gogs as a service. The file is in `/gogs/scripts/systemd/gogs.service`.
3. Note location where all Ubuntu services are located. It is located in `/etc/systemd/system/`.
4. Note your current user and group. You can check it using command `groups [username]`. In my case, my username is of course *ubuntu* and the group is *ubuntu* as well.

What you need to do now is to configure the Gogs app configuration to be like this:

```
...
# make sure to use correct user (I use ubuntu)
RUN_USER    = ubuntu
...
# is where your all Git repositories are located
ROOT        = /home/ubuntu/web/gogs/gogs-repositories
# point to your available domain, if you don't then it will be your public IP
DOMAIN      = example.com
# it is up to you to change the port for your server, I'm using default 3000 port
HTTP_PORT   = 3000
# since I want to use my main domain www.example.com for something else, I use a subdomain to host the Gogs
ROOT_URL    = http://gogs.example.com
...
```

Complete guidelines are available in well documented [Gogs doc page][s9]. Spend your time on this hacking cheat-sheet to master on Gogs.

Then, the next step is to run Gogs app as Ubuntu service. That means, everytime your instance is rebooting, Gogs will be reloaded and your site will online in no time. To do this, copy Gogs *systemd* service file to `/etc/systemd/system/gogs.service` and then edit it with root access and follow this example:

```
...
WorkingDirectory=/home/ubuntu/web/gogs
ExecStart=/home/ubuntu/web/gogs/gogs web -p 3000
...
Environment=USER=ubuntu HOME=/home/ubuntu/web/gogs
...
```

Don't forget to take the ownership of your Gogs exec file by calling `sudo chmod +x /home/ubuntu/web/gogs/gogs`. Now you can start the Gogs service by calling `sudo systemctl start gogs` and followed by `sudo systemctl enable gogs`. Make sure that Gogs' service is running by doing `sudo systemctl status gogs`. 

Whenever you made an update, you can reload the service by calling `sudo systemctl daemon-reload`.

# Step 6: Install Nginx

Install the Nginx by invoking `sudo apt-get install nginx`.

# Step 7: Secure access to your Gogs site

[s1]: www.namecheap.com
[s2]: lightsail.aws.amazon.com
[s3]: https://lightsail.aws.amazon.com/ls/webapp/account/keys
[s4]: https://gogs.io
[s5]: https://www.nginx.com/
[s6]: https://certbot.eff.org/
[s7]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/managing-users.html
[s8]: https://gogs.io/docs/installation
[s9]: https://gogs.io/docs/advanced/configuration_cheat_sheet