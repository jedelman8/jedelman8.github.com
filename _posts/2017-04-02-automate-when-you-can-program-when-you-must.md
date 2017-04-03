---
layout: post
tags: automation
title: Automate When You Can, Program When You Must
---

I've had a general thought I've wanted to write about for quite some time now and after just seeing Matt Oswalt's latest post [Learn Programming or Perish(?)](https://keepingitclassless.net/2017/03/learn-programming-or-perish/), the thought finally makes it to paper so to speak in this post.  The thought I want to expand on is something I say quite a bit as I talk about network automation.  It is _automate when you can, program when you must._

After reading Matt's post, I'll re-phrase to _automate when you can, script when you must_ specifically targeting network engineers (note: even though this is what I mean, the word _script_ makes it a bit clearer).  This is a twist on the network industry's old saying of _switch when you can, route when you must_.

# Automate When You Can

**_Automate when you can_** is saying use some form of tooling when you can to _do network automation_.  Why re-invent the wheel when you don't have to?  I'm a little biased these days, but this means using some form of extensible tooling, preferably open source, that _does automation_.  Some of my favorites right now are Red Hat's Ansible and Extreme's StackStorm.  However, this could just as well be other open source tools such as Puppet, Chef, or SaltStack, or even commercial solutions from vendors like Cisco, VMware, Arista,  and **insert favorite vendor**.   Ideally, you (your network team) are automating 80% of the time.

# Program When You Must

**_Program when you must_**, now re-phrased as _script when you must_ is saying, when the **tool** cannot do what you need it to do, or when the **tool** becomes too complex to perform a certain operation, script it, and when you script it, hopefully you're _extending_ the tool of your choice.  In Ansible, _scripting_ means you're writing a custom module or enhancing an existing one or writing a custom Jinja2 filter.  This is NOT hard-core programming that requires years of professional training as a software engineer.  You can script (yes, it's still programming) amazing things in 30-40 lines of code (maybe it's a little more or less).  We aren't talking about writing 1000s of lines of code though. Ideally, you (your network team) are scripting/programming 20% of the time.  

> Note: I used the word _programming_ in that last sentence because if you do have full-time _scripters_ on the team, they should be pushing themselves to get to the next level.

# The 80/20 Rule for Network Automation

The net result comes back to the 80/20 rule when looking at a team of current network engineers.  If you're on a team of 10 people, 8 of them should be using using a tool day to day.  If it's ~7, that's more than okay.  **Learning a tool to manage production environments is a challenge in itself**, forget about everyone learning _to write production code_.  Of these 7-8 people, there will likely be a few that script more than others for sure- that's the reality.  

Of the remaining 2-3 people though, they are scripting more frequently than the others.  These are the few that may straddle scripting and programming.   That said, the more the _scripters_ script, the more they realize they need to write better-quality code.  These folks realize that and _may_ make that gradual transition to worry about things like code review and automated tests.  This _could_ mean they evolve more into more of a software engineer in the long run.  However, that **DOES NOT** happen over night.  

# Becoming an Expert Over Night?

Did anyone with advanced network certifications get them over night?  Heck no.  It's a gradual process.  I often equate it to the CCNA, CCNP, and CCIE.  Does one learn BGP as a CCNA? Sure, I bet if you ask someone who just passed the CCNA about BGP, they can tell you all they know about BGP and think that's all they need to know.  Then they get to the CCNP routing book and realize there is so much more to learn; the process repeats again for the CCIE.  Oh, and this happens yet again for managing a production environment running BGP.  

So you're a network engineer without any automation/programming knowledge?  That's awesome.  Embrace it and learn as much as possible, but do not shoot for the _CCIE of programming_ tomorrow; go for the **NA** - learn to write small scripts, understand data types, and data formats!

# Network Automation Takeaways

First and foremost, realize there is no right or wrong path here, but let's summarize this a bit for some key takeaways:

As a network engineer...

  * If you're just getting started with automation, experiment with a few tools, and then pick one.  Then stick with it.
  * No matter the tool, you need to understand data types.  This is not   programming.  Seeing a curly brace or two is okay, but still not programming.
  * You should understand data formats like JSON regardless of your tool of   choice.
  * You should understand how to script to either a) get something done your tool can't do well or b) troubleshoot (diving into a stacktrace) c) extend your tool of choice such that a new task that your tool couldn't do is now easily integrated into workflows your tool does do.  The value here is now others on your team can use your integration/plug-in without programming.

# Final Thought

Try and de-couple personal learning objectives with and when managing production environments.  It doesn't make much sense to build a **FULL** tool from scratch (excluding APIs and/or front-ends) unless you're dedicating several full-time software engineers, and even then, is that the best approach?  If you're thinking along those lines, where do existing [open source] tools fall short for you?

Happy Automating!

-Jason

@jedelman8


