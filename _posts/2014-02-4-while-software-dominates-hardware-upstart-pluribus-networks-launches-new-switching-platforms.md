---
layout: post
title: While Software Dominates Hardware, Upstart Pluribus Networks Launches New Switching Platforms
tags: pluribus
---

If you haven’t heard, there is a new switch vendor in town – [Pluribus Networks](https://www.pluribusnetworks.com/).  That’s right.  In the new world where hardware is being dominated by software, there is an upstart that is trying to sell ASICs (along with their value added software, of course).  This actually isn’t too common these days.  Since Software Defined Networking (SDN) became the latest craze, the only startups going after major incumbents have been Plexxi and Pica8.  Before them, Arista. 

Note: I am not including software only companies that can run on bare metal switches such as Cumulus Networks.
After talking to Pluribus last week, it’s clear, their value add is in their software, not their hardware, but their customers wanted to buy the software and hardware together, so that will be their primary route to market.  That said, the switches are coming from ODM manufacturers and if customers can get better deals going direct for the hardware, Pluribus is more than happy to just sell software.  Pluribus has a few models of switches.  One based on Broadcom and all others based on Intel Fulcrum.  The Broadcom is an off the shelf reference design while the Intel Fulcrum switches are ODM, but spec’d out according to the Pluribus designs.  The main differences for the Intel platforms are CPU, memory, and storage – so basically, the assumption being made is that customers will be running applications on the switches in the future and they’ll need more resources to accommodate for these applications. 

Applications deployed on the switches will vary, but Pluribus has a few of their own already being used in production environments.  They include fabric capabilities and analytics.  The fabric capabilities show the power of their distributed operating system where you can log into any one switch and issue commands to make a change to the fabric or any given switch.  So, it’s one login, one logical control point without any SDN controller required.  The analytics application works in a similar fashion capturing top talkers and the like.  Running analytics on every switch, the stats are aggregated using the distributed operating system across every switch.  The stats for the fabric can be viewed from any given switch – the demo was pretty slick.

And if you don’t like their applications, you can load your own applications because they are giving full access to the Linux shell on each switch.  This is becoming common place these days, but to really take advantage of running advanced networking applications, Pluribus is betting on their new reference switch designs being needed.  With their architecture that makes the switch look more like a server (read more CPU, memory and storage), you’ll be able run NFV applications such as FW, IDS/IPS, and Load Balancing, if you desire that functionality of course. 

Network Virtualization vendors like NSX offer distributed firewalling capabilities where the enforcement point is in the hypervisor, but with enough horsepower in the top of rack switch, it can very well be the TOR switch itself.  Often times, network engineers don’t want to have to interface with the server teams or maintain their own servers for NFV.  While I’m not advocating for it, with a Pluribus model, they could provide the TOR and server for NFV applications that just happens to also be the same physical platform.  No servers needed.

So what else is worth mentioning about Pluribus?

* They built their own L2 stack, but are using open source Quagga for L3
* The entire fabric has a RESTful API – the API call can be made to any switch (cool, right?)
* They have an API called vFlow that leverages the fabric API.  It looks very similar to Arista’s DirectFlow, but with vFlow it is applied across the whole fabric, not per switch.  This is another example where they are leveraging their distributed OS rather than implementing a controller.
* Average list price for their switches is $25K.  Running applications like Fabric or Analytics will drive the cost up.  
* They already have some large production implementations.  CloudFare, for example, has Pluribus deployed in over 20+ locations globally.  That is impressive considering they are just launching today.

While it’s healthy to see competition in any market, I particularly like that vendors are making their equipment much more open and programmable.  That is a win for the customers in my book.

If you want to learn more about Pluribus, they are presenting at [Network Field Day 7](http://techfieldday.com/event/nfd7/) later this month.

Thanks,
Jason

Twitter: @jedelman8

