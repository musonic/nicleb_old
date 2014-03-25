---
published: false
---

## Replicate Your VPS - custom provisioning, bash scripts, permissions and more!

In the last post we looked at how to setup and provision your VM using Vagrant and Salt. In this post I'm going to dive a little deeper (only a little) and walk through how I have setup my VM to duplicate the VPS (Virtual Private Server) that I will be using.

Why do I need to do this?

Well, the whole point of using a VM for development is to make your development environment as close as possible to your production environment so that you don't get any nasty surprises when it comes time to deploy.

In my case I have signed up for a VPS with a company called [ServerGrove](http://servergrove.com/vps). I chose them because they had good reveiws especially from the Symfony2 community which is currently my PHP framework of choice, and certainly what I would be using on my next project. I have found their service brilliant so far. I think the pricing is competitive, their UI is really easy to use but best of all I have found their customer service and support to be second to one. I have honestly never experienced anything like the level of support that I've had from these guys. Total superstars.

So, in order to replicate my ServerGrove VPS as closely as possible I need to try and copy the following things:
1. Use the same OS / Base Box
2. Provision it with the same LAMP stack

The OS part is pretty straight forward. I'm using "precise64" which is a pretty good match, at least for the time being.

The interesting part is when we get to provisioning our LAMP stack.

### Let's Give It A Bash

ServerGrove usefully provide publically available repositories of all the software that they install on their VPSs. The question is, how do we use them?

If you read the Vagrant [docs](http://docs.vagrantup.com/v2/getting-started/provisioning.html) you will see that in their getting started tutorial they provision their first box by using a script that they call directly from their VagrantFile. This script is called a bash script and it effectively allows you to run the same commands that you would run in the command line. As such, it is really the simplest way of provisioning your VM, but it's not the most flexible. 

However, we need to run some commands on our VM _before_ we start to provision it in order for us to download the software from the ServerGrove repo and use that to provision rather than the defaults.

Start by creating a new file in your project root (the same level as your VagrantFile) and call it anything you like - I've called mine "bootstrap.sh".

Copy and paste the following into the file, but remember to read the explanation that follows carefully so that you understand what you've just done!

    #!/usr/bin/env bash
    
    apt-get -y install curl
    
    echo deb http://repos.servergrove.com/servergrove-ubuntu-precise precise main > /etc/apt/sources.list.d/servergrove.list
    
    curl -O http://repos.servergrove.com/servergrove-ubuntu-precise/servergrove-ubuntu-precise.gpg.key 
    apt-key add servergrove-ubuntu-precise.gpg.key
    
    apt-get update

The first line is important. The first two characters mark the beginning of the script. The rest of the first line is defining which shell will be used to run the commands. In our case it is Bash (Bourne Again SHell). If you would like to find out more about Unix Shells and Bash in particular take a look at [this beginners guide](http://www.tldp.org/LDP/Bash-Beginners-Guide/html/).
Next we make sure that [curl](http://curl.haxx.se/docs/faq.html) is installed.
The third command gets the contents of the remote repository and places it in a new file on our VM. This is where the ServerGrove versions of PHP, Apache etc. reside.
Next up we have to add a [gpg key](http://www.gnupg.org) for security.
Finally, we update apt-get so that it can now access the packages contained in the ServerGrove repository.

I'm hoping that I will be able to move all of this stuff into my SaltStack configuration but I haven't worked out yet how to do that. The main thing is that this works!

Next is provisioning...

###Salt and Symfony

At this point it's worth recapping exactly what I am trying to achieve. As well as trying to replicate my ServerGrove VPS as closely as possible I am also needing to prepare my VM for using the [Symfony2 PHP framework](http://symfony.com). Symfony2 has certain [requirements](http://symfony.com/doc/current/reference/requirements.html)which take a little bit of extra configuring on most systems but the SaltStack options I'm about to explore will be useful and resuable regardless of what you are trying to do with your LAMP stack.

###PHP extensions and config

Let's make a start by setting up PHP.

The first thing we need to do is to make sure that we are installing the intl extension because this is required by Symfony2. 

The installation part is very straightforward, the enabling part less so! To install the extension you simply need to open up your php55.sls file and add the package name php55-intl to your list of packages. The full file should now look like this:

php55:
    pkg:
        - installed
        - pkgs:
            - php55
            - php55-apcu
            - php55-mod-php
            - libjpeg-turbo8 
            - php55-intl