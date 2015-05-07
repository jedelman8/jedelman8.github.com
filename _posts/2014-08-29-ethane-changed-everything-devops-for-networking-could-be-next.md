---
layout: post
title: Ethane Changed Everything - DevOps for Networking Could be Next
tags: ethane, sdn, openflow, startup
---

It’s an interesting time in networking, isn’t it?  I can probably quote myself saying that for as long as I’ve been blogging and about a year before that.  Supposedly 2015 is the year of POCs, bakeoffs, and seeing which startups continue to get funding, and which ones slowly dissolve.  As we start to see who the winners and losers may be, I thought it would be good to highlight the last 7 years and where the major focuses areas have been and see what could be next.

### Hello OpenFlow!

By now, many of us know who Martin Casado is and what he’s done.  His PhD work, [Ethane](http://yuba.stanford.edu/ethane/), at Stanford with Nick McKeown and team led to the pre 1.0 work of OpenFlow.  For the first several years of the network (r)evolution, it was all about OpenFlow.  By 2009, the phrase Software Defined Networking had emerged and referred to OpenFlow enabled architectures.  It was easy to understand.

* As the industry chatter increased on OpenFlow architectures, hardware commoditization, and the de-coupling of the control plane and data plane, Casado had already started Nicira with McKeown and Shenker.
* When limitations were seen on what OpenFlow (at the time) could due in hardware and with the emergence of open source server virtualization, it’s believed that Nicira had a pivot and started down the path of network virtualization.  The first step here was creating Open vSwitch.  This is when OVSDB came out too.  For years, Martin Casado would never refer to the Nicira product, NVP, as SDN.  From a personal perspective, I admired that.  It was about network virtualization – the solution.  It was about de-coupling the virtual network from the physical network offering flexibility, mobility, control, and *choice.*  
* Conversation and debate started and continued on overlays.  Types, standards, which one was best, etc.  It was really fun stuff.  They were even using CAPWAP.
* It might be at about this time any company who was selling software that performed a network function became SDN.  Many of us clarified.  Linerate and Vyatta were not software defined, but rather software enabled.  This was fun too.
* By now, the Nicira team was already working on a more robust networking plug-in for OpenStack too.  Let’s not forget Quantum.
* Everyone else was still trying to figure out if OpenFlow could scale.
* SDN-washing was well underway.  The best comparison for me was the number of vendors at the first ONS I attended in 2011 and then at the third in 2013.  One executive said at the 2012 conference SDN stands for “still don’t know.”  
* In this time period from 2008-2011, there was an influx of VC/vendor money into the network industry.  There was Big Switch, Plexxi, Embrane, Cumulus, Pica8, Midokura, Contrail, PLUMgrid, Nuage, and Insieme.  This doesn’t even include some new comers like Avi, Glue, Viptela, vArmour, and the list goes on. 
* We actually have to include Arista too because they’ve always been doing SDN.   APIs soon took over the conversation.  Does every vendor have an API?  Is it open?  That’s also SDN.  
* Okay, this meant if a product had an API, controller, or overlay, it was pretty much SDN.
* By mid-2012, Cisco funded Insieme.  Everyone had an opinion on what Insieme was going to do.  It was high density 100G.  It was converged networking with storage native on box, but, no matter what it was, it was going to be Cisco’s version of SDN.
* VMware acquires Nicira for $1.2B.   Need I say more?
* In early 2013, some power moves were made and OpenDaylight emerged at that years ONS.  Controllers were already becoming a commodity.  This would be the platform that unifies different vendors’ solutions.  It’ll have every protocol supported southbound and have open REST APIs northbound.
* Late 2013 came around and as expected, Cisco ate up Insieme and brought MPLS-TE back on board.  That is, of course, Mario, Prem, Luca, Sonia, and Tom Edsall.  ACI emerges.  It’s about Application Centricity.  Companies rally around “AC.”  Declarative state, promise theory, and policy all become a thing in the network world.
* The on-box API trend slowly morphed into network engineers needing to be programmers.
* The whitebox trend got revived with Cumulus.  It became more about *choice.*  This time, it was choice without a SDN controller.  Go figure, but it still gets thrown into the SDN bucket.  The whitebox trend quickly turned into the bare-metal trend when Dell announced support for BigSwitch SwitchLight and Cumulus Linux on their hardware.
* I admit, I’m writing this from memory and it’s getting hard to keep track, so I’ll stop trying to get it in chronological order.
* Other notes to highlight are ACI going GA, NSX going GA, OpFlex, and Policy/Congress in OpenStack.  And of course ETSI emerged and NFV became a real thing.  As of late, one can even say NSX has pivoted around security.  But when you look at Ethane, should it be a surprise?  Either way, there are real customer needs around security – almost all environments I’ve seen are pretty wide open out there once you’re on the network.

On a side note, I remembered downloading a bunch of docs from Stanford's site in 2010 when I first started researching OpenFlow.  Before publishing this post, I went to take a look at the Ethane presentation and what surprised me was the following slide on Policy.  Don't forget to check out the date :)  Impressive, very impressive. 

![policy](/img/policy.png)

It’s an exciting time in networking, isn’t it?  I can probably quote myself saying that for as long as I’ve been blogging and about a year before that.  Supposedly 2015 is the year of POCs, bakeoffs, and seeing which startups continue to get funding, and which ones slowly dissolve.  As we start to see who the winners and losers may be, I thought it would be good to highlight the last 7 years and where the major focuses areas have been and see what could be next.

### Hot Topics

If we trend out the technologies/architectures of the past 7 years we have the following:

* OpenFlow
* Software Defined Networking
* Whitebox switching
* Network Virtualization
* Overlay protocols
* Open vSwitch
* OpenStack Networking
* Programmable Network Devices with APIs
* Network Functions Virtualization
* Bare metal switching
* Tightly coupled fabrics (controller + P + V)
* OpenDaylight
* Policy

### Now what?

Enterprises have solutions to buy now.  If you look at ACI and NSX as two examples, they are platforms – solid platforms. Integration efforts with L4-7 partners, white list approach to data center networking – it’s all good.  The OpenFlow and overlay protocol debate is pretty much gone.  The DIY and MSDC crowds are looking at OVS, OpenStack, and bare metal/whitebox, or just simply building their own.

### The Next Trend

*Please read this as just ONE of the next trends…*

Vendors are still emerging and “cleaning up” their device APIs.  Almost every network device and for damn sure, every network (SDN) controller, has documented…dare I say open, APIs.   There is going to be a mix of controller networks getting sold and deployed going forward.  There is also still billions of dollars of hardware out there that is NOT in the data center.  There will be overlay/underlay integrations and there will be public cloud integration efforts.

If we look back at the technology trends above or even any of the listed startups, and ask why they emerged, answers are usually around agility, security, reduced CapEx, and/or reduced OpeEx.

This begs the question, how does one see the results wanted ("the why") across the network that is multi-vendor, multi-technology, with or without a controller, and spans to the public cloud, and beyond.

One possible answer is **DevOps for Networking**.  The DevOps community is about 5 years old and we, as the network industry, can learn a tremendous amount from them about culture, community, sharing, and the tools they use.

We are at ground zero with the integration between DevOps and networking although there have been recent announcements like Puppet getting another round of funding to focus on networking and companies like Cumulus and Juniper pushing modules upstream to Ansible Galaxy.  All good stuff.

### How to get started?

[There is a first of its kind event happening October 14, 2014 in Mountain View](http://www.devops4networks.org/).  It is a 1 day meet up to hear and learn about DevOps for Networking.  The speaker line up looks amazing already.  Check it out – it was just announced this week.  Hope to see you there!

BTW, feel free to add to the timeline above by commenting below.  This was from memory, so I’m sure I missed a few things :)

Thanks,
Jason

Twitter: @jedelman8
