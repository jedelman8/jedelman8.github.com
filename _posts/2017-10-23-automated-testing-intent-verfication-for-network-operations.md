---
layout: post
tags: network automation, ci/cd
title: Automated Testing & Intent Verification for Network Operations
---


The most important part of writing quality software is testing.  Writing unit tests provide assurance the changes you're making aren't going to break anything in your software application.  Sounds pretty great, right?  Why is it that in networking operations we're still mainly using ping, traceroute, and human verification for network validation and testing?

# The Network is the Application

I've written in the past that deploying configurations faster, or more generally, configuration management, is just one small piece of what network automation is.  A major component much less talked about is automated testing.  Automated testing starts with data collection and quickly evolves to include verification.  It's quite a simple idea and one that [we](http://networktocode.com) recommend as the best place to start with automation as it's much more risk adverse to _deploying configurations faster_.

In our example, the network is the application, and _unit tests_ need to be written to verify _our application_ (as network operators) has valid configurations before each change is implemented, but also integrations tests are needed to ensure _our application_ is operating as expected after each change.  

# DIY Testing

If you choose to go down the DIY path for network automation, which could involve using an open source framework as a foundation, e.g. Salt, Ansible, Puppet, you may also go down the path of writing tests manually.  The types of tests that you should be using for production network changes are limitless from verifying neighbors to the quantity routes to even more basic things such as MTU consistency and the enforcement of Enterprise standards for names and descriptions.  Writing these tests to verify your intent is no easy feat.  It'll take time and dedicated resources for each part of your network.  

# Commercial Testing Platforms

On the other hand, if you prefer to have a hand to shake, or throat to choke, you may also want to consider platforms such as Forward Networks or Veriflow.  I recently saw Veriflow at the latest Networking Field Day and Forward at ONUG.  Veriflow has dozens of built-in consistency and intent-checks--they basically collect the output of operational show commands and offer a platform to define automated tests/checks against that data.  If I must admit, Veriflow was one of my favorite presentations of the NFD event.  It's worth noting that everything they offer is also exposed via a RESTful API. 

# From API to Platform Integrations

These types of vendors that are focused on intent-based verification need to provide more than an API though.  In conversations with both companies, the focus is using the UI and doesn't fully reflect an API-first strategy.  We haven't quite crossed the chasm of network automation going main stream, but as they say, skate to where the puck is going, not where it is.  What do I mean by this?

In the network automation space, the puck is heading to leveraging CI/CD pipelines for network operations.  This means companies need integrations, not just APIs (same as I've said in the past about network vendors touting APIs).  In this case, companies have the opportunity to be CI platforms for the network.  This could be in the form of git integrations, e.g. just like Travis has a native plug-in to GitHub.  Think about describing your tests in a YAML config file stored in a repo that triggers Veriflow tests when ever a pull request is opened.  Think about doing a deployment with Ansible, but having Ansible trigger post-deployment verification tests (if you aren't already writing your own tests with Ansible).  

# Closing 

The truth is all of that is possible today, but wouldn't it be nice if there were native integrations available so that everyone wasn't re-inventing the wheel? 

The network automation space and its surrounding ecosystem of tools is still in its infancy and I truly look forward to its future.

Thanks,

Jason (@jedelman8)










