---
published: false
---

##Provisioning

You could do this manually but downloading whatever you need using package managers like apt-get etc. This is fine, but there are two important reasons why this probably isn't the best way to go.

1. To perform tasks like installing new software you will have to have root access. At the moment you are logged in as a user called Vagrant, not Root. You can change this easily enough, however, by typing:

	sudo su
    
2. This is the biggie. If you install things by hand then when you destroy your virtual machine (which you will do, often) then everything you have installed will disappear with it. You will have to re-install everything _every_ time you startup a new VM. Bad times.

So what's the answer? It's called **Provisioning** and it means that you have some sort of config file saved that Vagrant can run a series of commands and install everything you need _each time you start a new VM_. 

Unfortunately, this is where things start to get (for me at least) a bit tricky. There are various options, but it took me quite sometime to get this setup correctly.

From what I can gather there are three main ways of provisioning a VM using Vagrant (not including writing a bash script - see the vagrant docs for info on this):

1. Puppet
2. Chef
3. Salt

I think that they all do basically the same thing, but in slightly different ways. I've tried playing with Puppet but whilst asking for some help on the Vagrant irc channel I was recommended Salt which is what I'll be talking about now. Puppet and Chef both use Ruby, so if that's your thing then go for that. Salt uses files written in the YAML format which I personally find much easier to understand. It is also used a lot in Symfony2 and Doctrine which I will be discussing later on in this series.
