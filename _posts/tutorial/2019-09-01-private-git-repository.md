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

**Note** This will let you as the owner of the instance to connect to your instance and has all root access. It is, in fact, not a good practice. You may want to have multiple people working on your instance, one for this project and one for others. Thus, it is good to create an another user just to work for this tutorial. You can just follow the guide on it [here][s7].

# Step 3: Install necessary components

Update your instance's Ubuntu by calling `sudo apt-get update`. This will take a while, but usually it's good time to brew your favorite coffee. 

Once you finished your coffee, you might want to know what are the components we need to install. If you have time, it's always good to read about their pages a bit. So, here is the list:

* [Gogs][s4]. A painless self-hosted Git service
* [Nginx][s5]. True magic here
* [Certbot][s6]. To enable secure SSL/HTTPS in your web server




[s1]: www.namecheap.com
[s2]: lightsail.aws.amazon.com
[s3]: https://lightsail.aws.amazon.com/ls/webapp/account/keys
[s4]: https://gogs.io
[s5]: https://www.nginx.com/
[s6]: https://certbot.eff.org/
[s7]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/managing-users.html