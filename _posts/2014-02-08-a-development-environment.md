---
published: false
---

## Getting Started - a basic development environment

So, before we can do any exciting coding it's good to get yourself setup and feeling comfortable. It's a bit like cooking a new recipe and making sure you've got all of your ingredients out on the counter before you start rather than scrabbling around at the back of a cupboard once you're halfway through.

I will put a reference at the end of each article with relevant links. I will also try and use inline links where appropriate and if there are technical terms (or even semi-technical terms) I will try and link to Wikipedia articles.

In this post I'm going to guide you through how I setup my current development environment. Please remember that:
1. This is just how I do it - there are a million and one ways to do the same thing.
2. This is only a basic setup. All of the things I talk about can be taken much, much further if you have the time, inclination, mental capacity etc.
3. IMPORTANT - I develop on a Mac running OS X. If you use a different operating system then I cannot guarantee that what I am doing will work without some modification. I would like to think that the fundementals will be the same, but you might have to do some googling to help!

<section id="aims">
### Aims

By the end of this article we should have:
1. A working development environment to enable us to write and test code.
2. Understand the basics of a good workflow>
</section>

<section id="what">
### What is a development environment?

An environment is a way of seperating different stages of building a web site or app or whatever. At it's most basic level you will have a developement environment and a production environment. The development environment is where you'll spend most of your time and is usually on your local computer. This is where you write your code and check that everything works before releasing it to the wild. The production environment is where your code is when your site is live. This usually involves hosting of some kind unless you're able to run your own servers. 

Really you can define any number of environments in any way that you like. It depends on what works best for your workflow. The most common additional environment is often called the Staging environment. This is like a half-way house between development and production and is usually used for rigorous testing before the code is finally pushed up to the production server.

I'm going to show you how to setup two environments: developement and staging. Production environments depend on how you want to host your work so I will leave that for a later post.
</section>

<section id="setting-up">
###Setting Up

There are many ways to setup a development environment. At the most basic level you need:
1. a local webserver
2. a database server
3. a scripting language
4. a way of editing files
This is often referred to as your development stack. You will also see this abbreviated to *AMP. The star could be W for windows, M for Mac or L for Linux. The A stands for Apache (the webserver), M for MySQL (the database server) and P for PHP (a scripting language).

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

A Virtual Machine is software that runs on your computer and emulates a completely seperate operating system.





</section>