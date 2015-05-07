---
layout: post
title: Using Schprokits to Automate Big Switch's Big Cloud Fabric
tags: schprokits, big switch
---

I gave a [presentation](/home/interop-nyc-software-gone-wild) at Interop last month and tried to make two major points about network automation.  One, network automation is so much more than just “pushing configs” and two, network automation is still relevant in Software Defined Network environments that have a controller deployed as part of the overall solution.  And I’m referring to controllers from ANY vendor including, but definitely not limited to Cisco’s APIC, NSX Controllers, Nuage Controller/Director, Juniper Contrail, Plexxi Control, OpenDaylight, and Big Switch’s Big Cloud Fabric.  

A few months ago, I was at [Network Field Day 8](http://techfieldday.com/event/nfd8/) and got to see a live demo of Big Switch’s newly released Big Cloud Fabric solution.  It seemed slick, but I was curious on automating the fabric using the northbound APIs exposed from their controller.  As it turns out, I was able to get access to a small fabric (2 leafs / 2 spines) to get familiar with Big Cloud Fabric.  In parallel to that, I started testing Schprokits as I mentioned in my [previous post](/home/automated-network-diagrams-with-schprokits-autonetkit).

So, sure enough I spent some time putting together a demo to show what can be done with network automation tools and how they could integrate with SDN controllers.  The result of this work can be seen at the [YouTube Video](https://www.youtube.com/watch?v=ytv1F39fT7I) below.

To clarify, I had access to Schprokits and access to Big Switch kit.  Schprokits does not natively support Big Switch integrations (maybe in the future now??).  I worked with the Schprokits team to understand “how to extend” Schprokits and how to create custom actions.  I then worked with Big Switch to ensure I understood their API.  What's pretty cool is that their CLI uses their API and there is even a command to debug/see the APIs being executed while you are the command line.  But, an important point is the extensibility of each platform.  To extend Schprokits didn’t mean having them write the code or wait for them to get it on their product roadmap.  It meant, as a user, if they don’t support a certain vendor, capability, or action, you have the power to extend the functionality (by the way, this is where coding comes into play).  So, that’s what I did.

#### [YouTube Video](https://www.youtube.com/watch?v=ytv1F39fT7I)

As you watch the video, take note and remember a few things:

* This can be done with any piece of network equipment or SDN controller.  If there are APIs, it makes it much easier.
* From my perspective, this type of work is hugely valuable for vendors and integrators who need to stand up demos, POCs, general deployments, and even training labs.  Imagine there being a class with 20 labs and then a few months later you want to test just lab 18.  Ugh.  Automate labs 1-17 and then get to 18 rather than waste time doing 1-17 repeatedly.
* And don’t forget, I only show a glimpse of what can be done here in terms of data extraction in the demo, but you can create reports, get any data you want from the controller or devices, etc. 
* This could simplify DR and site re-location.  As you templatize your configs and automate them, you can easily bring up a DR network or SDN controller in seconds to mimic prod!  
* Remember, even with controllers, you have change windows and in those change windows, you may be clicking around a GUI and fat finger something.  That can be eliminated.

Want me to mock something up (time permitting of course) similar for your controller platform?  Let me know and maybe we can figure something out.  At the very least, I can show you how I did what I did for the Big Cloud Fabric Schprokits actions (and Ansible modules not shown).  

Have any comments? Feel free to write in below!

Thanks,
Jason 

Twitter: @jedelman8
