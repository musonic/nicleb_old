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

### Salt Stack
So, let's get going with Salt. Much of what follows is based on work done by [Geoff Petrie](http://geoffpetrie.com) and his freely available github repository to build a PHP Lamp stack using Vagrant and Salt that you can find [here](https://github.com/geopet/salt-lamp-vagrant). If you like you can follow the instructions in his github repo readme and you'll have a fully working LAMP stack, however if you want to walk through with me and learn what he's done, then carry on reading.

The good news is that Vagrant (as of version 1.3) has native support for Salt. If for some reason you're running an older version either upgrade or install the vagrant-salty plugin. Google will help you out with either of these two things.

Next up, create a new folder to keep all of our SaltStack stuff and a subfolder called "roots".
	$ mkdir salt
	$ cd salt
    $ mkdir roots

Now let's think for a moment what we are trying to achieve. We need to set up Salt so that everytime we start a new VM using Vagrant we know that the new machine will be automatically provisioned with everything _exactly_ how we've specified previously. There's a lovely description about this in the [SaltStack docs](http://docs.saltstack.com/ref/states/index.htm):

> State management, also frequently called software configuration management (SCM), is a program that puts and keeps a system into a predetermined state. It installs software packages, starts or restarts services, or puts configuration files in place and watches them for changes.

Salt uses files called SLS files to store states. You should think of states as simpe data lists. This is why they are written in YAML format. If you haven't come across YAML before it is a super easy way of writing organised data lists. We need to write a separate state for every component that we want our VM to be provisioned with. Let's start with a nice easy example. We're going to write the SLS file for our Apache2 installation.

Open up your IDE or text editor or whatever and create a new file in the roots directory and call it "apache2.sls". Then copy this:

	apache2:
  		pkg:
   			- installed
  		service:
    		- running

Please make sure you have indented the lines correctly because this is fundemental to how YAML works.

The first line is, as you would expect, the name we have given to this set of data. Officially it is called an _ID Declaration_. It is important that this matches the package name that you are going to install. This is dependent on your operating system and package manager. We are using Ubuntu so if you are using something else then you need to check what the package name needs to be.

The second line is the _State Declaration_. This tells us what state we are going to use. These are all listed in the docs and you can even create your own, but let's just stick with what we need for now. You can see now why the indentation is important. By indenting you can clearly see what data is a subset of something else. In our case "pkg" and "service" are indented by the same amount so they are obviously first children of "apache2". I hope this is fairly self-explanatory.
The state declaration refers to a salt state module. This has several functions built in that do various different things. The third line in our example simply says that we want to use the "installed" function of the "pkg" state module. As you would expect this simply is a check to make sure that the package is installed.

Moving on you can see we use another module called "service". Here we've specified that apache should be started if it is not already running.
And there we are, you've just written your first SLS!

The next step is to create a file called top.sls which will work as a master mapping file. Top files come into their own when you want to set up different environments or provision different minions separately. For us it will be very straightforward. Once you've created the file (in the salt directory) copy in:

  base:
    '*':
      - apache2
  
Here we have created an environment called "base". Within this environment we are using "\*" to match all minions (if you have mulitple minions you might specify specific names instead) and finally we send all minions the apache2 state. Pretty easy, huh?!

We just have one more file to create. In the salt directory created a blank file (make sure it isn't automatically given the .txt suffix) called "minion". This is a config file and all it needs in it is:

	file_client: local

By default Salt looks for files on the master server, but because we are going to utilise Vagrant's ability to sync folders.

The final step is to update our Vagrantfile. Open up your Vagrantfile and add:

	  ## For masterless, mount your file roots file root
  		config.vm.synced_folder "salt/roots/", "/srv/"
  
    # Provision using Saltstack
    config.vm.provision :salt do |salt|
  
      ## Minion config is set to ``file_client: local`` for masterless
      salt.minion_config = "salt/minion"
  
      ## Installs our example formula in "salt/roots/salt"
      salt.run_highstate = true
  
    end
 




