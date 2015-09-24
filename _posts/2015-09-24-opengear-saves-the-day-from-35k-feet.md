---
layout: post
tags: opengear
title: Opengear Saves the Day from 35K Feet
---

On Tuesday, I was boarding a flight heading to the west coast and realized I had 3 switches powered down in the colo that I needed for a presentation and demo on Wednesday.  Not a good feeling.

We have an IP enabled PDU that I usually connect to with no issues from the office and home office since we also have an SD-WAN deployed using Viptela.  The only issue --- there is not a way to VPN into the colo (yes, that's my fault).

As it turns out, I had exposed the Opengear console server, that we used for all out of band access, to the Internet a few months prior.  On a flight that I was only planning to do offline work, I was forced to purchase Wifi...from there, the fix was pretty simple.  

I SSH'd into the Opengear console server, got access to the Juniper SRX perimeter FW, and then added a temporary NAT configuration that exposed the PDU to the Internet.  I was able to now access the PDU directly from 35K feet in the air, get the devices powered up, and have some peace that they could be used in the demo.  Sure, I could have opened a ticket with the colo and had them do it, but what kind of blog post would that have made?!? 

So, the point is never underestimate the need for out of band console access.  You never know when it will be needed.  I say this because I've supported A LOT of customers that have ZERO OOB access to devices.  Opengear saved the day for me, and maybe one day, they can for you too!

And the cool thing is this post is getting published on the way home, while also from 35K feet!


Thanks,
Jason

@jedelman8



