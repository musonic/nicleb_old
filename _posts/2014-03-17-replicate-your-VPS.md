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

### Let's Provision Again

ServerGrove usefully provide publically available repositories of all the software that they install on their VPSs. The question is, how do we use them?


