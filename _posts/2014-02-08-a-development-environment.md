---
published: true
layout: post
date:
tags: Web Development, VM, Vagrant
---

## Getting Started - a basic development environment

So, before we can do any exciting coding it's good to get yourself setup and feeling comfortable. It's a bit like cooking a new recipe and making sure you've got all of your ingredients out on the counter before you start rather than scrabbling around at the back of a cupboard once you're halfway through.

I will put a reference at the end of each article with relevant links. I will also try and use inline links where appropriate and if there are technical terms (or even semi-technical terms) I will try and link to Wikipedia articles.

In this post I'm going to guide you through how I setup my current development environment. Please remember that:
1. This is just how I do it - there are a million and one ways to do the same thing.
2. This is only a basic setup. All of the things I talk about can be taken much, much further if you have the time, inclination, mental capacity etc.
3. IMPORTANT - I develop on a Mac running OS X. If you use a different operating system then I cannot guarantee that what I am doing will work without some modification. I would like to think that the fundementals will be the same, but you might have to do some googling to help!

### Aims

By the end of this article we should have:
1. A working development environment to enable us to write and test code.
2. Understand the basics of a good workflow


### What is a development environment?

An environment is a way of seperating different stages of building a web site or app or whatever. At it's most basic level you will have a developement environment and a production environment. The development environment is where you'll spend most of your time and is usually on your local computer. This is where you write your code and check that everything works before releasing it to the wild. The production environment is where your code is when your site is live. This usually involves hosting of some kind unless you're able to run your own servers. 

Really you can define any number of environments in any way that you like. It depends on what works best for your workflow. The most common additional environment is often called the Staging environment. This is like a half-way house between development and production and is usually used for rigorous testing before the code is finally pushed up to the production server.

I'm going to show you how to setup two environments: developement and staging. Production environments depend on how you want to host your work so I will leave that for a later post.


###Options

There are many ways to setup a development environment. At the most basic level you need:
1. a local webserver
2. a database server
3. a scripting language
4. a way of editing files

This is often referred to as your development stack. You will also see this abbreviated to \*AMP. The star could be W for windows, M for Mac or L for Linux. The A stands for Apache (the webserver), M for MySQL (the database server) and P for PHP (a scripting language).

You can see that this doesn't sound complicated or difficult to setup. The other great news is that you can setup a really good development environment completely free (except your hardware). 

As far as I know there are three main ways you can go about setting up a development environment and there is quite a lot of crossover between them.
I'll briefly explain each in turn and then go into more detail about my current preferred option.

####Using pre-installed software plus some extras
If you are using Mac OS X then there is a ready-made development environment for you. The OS comes bundled with an Apache web server, MySQL and several scripting languages including PHP and Ruby. You can use TextEdit to edit and create files or, if you feel comfortable, a command line _(Terminal in Mac speak)_ editor like Vim. This works absolutely fine and if you choose to you can modify, upgrade and basically pimp the setup to your hearts desire. Some things you might want to do are:
- Manage your software using a package manager. Homebrew is one of many.
- Download a better way of creating and editing your code. This could be an editor like TextMate or maybe a full IDE like NetBeans or Aptana.
This is how I have been developing for several years.

####Download an environment tool
The most common tool for helping you build and manage environments is MAMP. This is software that you download and it gives you a ready made MAMP stack. This is _really_ easy to use. You make sure that your code goes in the correct folder and bamm everything is setup for you. You can tweak settings etc. and it just works. The benefit of this is it means you don't have to mess around with your OS to get it all going. There are plenty of other options for this kind of installation which I'll add links to at the end.

####Use Virtual Machines (VMs) and a VM management tool
So, this is where I'm now at. This is still a very recent departure for me and I am still tweaking and getting things right, but the good news is that you can dive straight in with me!
In the past I always shyed away from virtual machines. I couldn't really see the benefits, I was frankly a little bit scared in case something went wrong and also I didn't really want to spend the cash!
It turns out I was wrong on all counts...
Firstly, it doesn't have to cost you anything. Secondly, there is nothing to be scared about - if you don't like it you can just uninstall everything just like any other piece of software. Thirdly, I can now _really_ see the benefits. Let's start with some basics.

A Virtual Machine is software that runs on your computer and emulates a completely seperate operating system. This is great because it means that you can set it up to replicate your production server which will often be different, especially if you develop using a Mac. If your development server is effectively the same as your production server then it should remove that commonly heard excuse "But it works fine on my local machine" when things go wrong. It gives you the means to thoroughly test everything before you get to that point. 
The other huge advantage is that you can keep everything seperate from your local operating system. You can install, try out, mess up as much as you like and if you want to you can remove the whole thing with one command.

So, now that I've convinved you that this is the way to go, let's get started setting things up. Remember that everything I write assumes you are using Mac OS X simply because that is what I use and I don't have access to anything else. 


###Setting up your VM
In order to follow along you will need to use the Command Line. For some people this can be scarey. It looks very nerdy and not dissimilar to The Matrix. It is also _very_ powerful and so you are right to treat it with respect and caution. Make sure you understand each and every command you type into it before you hit enter. Soon you'll realise that with this power comes flexibility and speed and you will start to wonder why you were scared in the first place!
On a Mac the command line is accessed by using Terminal.app. You will be able to find this in you Applications folder inside the Utilities folder. Go ahead and load it up. You will see a black screen with something like this:

	Nics-MacBook-Air:~ niclebreuilly$
	
This is the name of your computer followed by the name of the currently logged in user. It also tells you that you are in the users home directory. You can check this by typing:

	$ ls

and hitting enter.
NB. The dollar sign ($) is used to denote a new command. You DO NOT type it again as you should see it already in your Terminal window. 
You should see a list of all the directories contained within you home directory. "ls" is the command for list directory contents. If you type:

	$ ls -ln
	
and hit enter you will get the same list but this time in a more verbose form with lots more information about the individual folders. "-ln" is a flag that is a combination of two options. The first is specifying a long listing format i.e. more information and the second specifies you want to see a numeric representation of user and group IDs. Try typing:

	$ ls -l
	
and hit enter and you'll see the difference.
Incidentally it is possible to customize the look of your Terminal window. Just go to the Preferences menu and you can select different themes for your windows. Select something you like and open a new tab or window. This will immediately make it feel more approachable.

Next we're going to start really getting our hands dirty.

In order to manage our VM we are going to use some software called Vagrant. Like everything in this series if you want to really get into Vagrant then you should check out their documentation.

First things first, we need to install it. Head over to http://www.vagrantup.com/downloads and select the correct download for whichever OS you are using and follow through the installation process. It is incredibly simple.

Once you have the software installed, go back to your Terminal window and type:
	
	$ vagrant -v
	
(from now on I'm going to assume you know to press Enter each time you type a command to make it run). If everything has installed correctly it should print out the version of Vagrant that you now have installed on your machine.

Next we need to download and install our Virtual Machine software. I will be using the free and open-source VirtualBox by Oracle. There are others available but VirtualBox is free and does everything we need it to. Head over to https://www.virtualbox.org and download and install the appropriate version.

With these two bits of software installed we're ready to setup our first project.

###Create a new project
Ok, so we first need to create a folder for our new project. You can do this through Finder or why not use the command line? For now, I'll assume you're going to add your project folder directly into your Home folder (although you can, of course, put it whereever you want!). So type the command to make a new directory:
	
	$ mkdir my_web_project
	
This will create a new directory called "my_web_project" in your Home folder. Open up a new Finder window and check!
Now you need to navigate into that folder. This is done with:
	
	$ cd my_web_folder
	
You can use this command to navigate yourself around your file system. You can use paths to get to specific folders rather than a new command each time. If you want to go up a level you would type:

	$ cd ../
	
If you want to start again from the user's Home directory then type:

	$ cd ~/
	
or you can type out the full path eg.
	
	$ cd /Users/niclebreuilly/

Now that you have your project folder you need to tell Vagrant that you want to use it. Make sure you're in your project folder.
	
	$ vagrant init
	
This will place a file called a vagrantfile inside that folder. This file is a configuration file where you tell Vagrant everything it needs to know about your setup. Open it up in a text editor or IDE of your choice and take a look. It is pre-filled with lots of comments and demo settings to give you an idea of what you can set.

Our next job is to setup the VM itself. Virtual Machines are often referred to as Boxes. There are loads of diffrent boxes you can choose from but for now we're going to use one of the default boxes that comes packaged with Vagrant. It is called precise32 and is a standard Linux Ubuntu 12.4 server.
	
	vagrant box add precise32 \ http://files.vagrantup.com/precise32.box
	
This downloads the box from the Vagrant website. 

The cool thing about this is that the same box can be used for multiple projects. What happens is that each time you setup a new project that project actually uses a clone of the original box and so you can have as many as you like!

Next we need to tell Vagrant to use the box. Open your Vagrantfile (if it isn't already) and find the line that starts
	
	config.vm.box
	
You need to un-comment it by deleteing the # symbol (if there is one) and then change it to

	config.vm.box = "precise32"


