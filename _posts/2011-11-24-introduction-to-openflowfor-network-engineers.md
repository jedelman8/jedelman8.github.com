---
layout: post
title: Introduction to OpenFlow for Network Engineers
tags: openflow, sdn
---

There have been numerous articles, blogs, and columns written over the past few months on OpenFlow and Software Defined Networking, but I still feel like many of them aren’t breaking it down for the typical Enterprise Network Engineer.  Having followed OpenFlow since mid 2010, I do not claim to be an expert in this space, but I will give my take on what could be the game changer for the networking industry.

Let me also apologize up front because this post is longer than I originally intended it to be!

### What is OpenFlow?

OpenFlow is simply a protocol that is used to communicate between a server, i.e. controller and network switches that allows the switch control plane to be separated from the data plane.  Now what does that mean and what is a controller?  This is where there are many analogies being made on what this actually looks like from a low level.  The most common comparison made is that it’s “the x86 instruction set for networking.”  For those academics or developers out there, this may mean a lot, but for me, it means absolutely nothing, and I consider myself pretty in tune with the mind set of someone designing Enterprise Data Center and Campus Networks, with the latest technology out there.  For me, the analogy that I use when explaining OpenFlow is directly relating it to the past and present Enterprise WLAN market.

### OpenFlow Networks vs. Enterprise Wireless Networks

How do we compare OpenFlow to WLAN?  Easy.  In my opinion, OpenFlow is to network switches as CAPWAP/LWAPP is to wireless access points.  Both are protocol specifications that allow for the separation of the control plane and data plane and are specifically used to communicate between a “controller” and network device (switch or AP).  If you’re familiar with the Enterprise WLAN market, you’ll also notice that Aruba came out with LAN switches last year that are managed via the same controller that is used to manage their APs; I presume using CAPWAP.  Pretty neat I’d say, but I have no idea what kind of traction they are seeing in the market.  If anyone knows, feel free to comment!

Note:  For those that actually wrote the OpenFlow and CAPWAP protocol specifications, I’m sure there are differences, but from a high level looking in, they are similar enough to get my point across.

### Where did OpenFlow come from?

Not that we’ll care in 2-3 years, but it may be good to know where and why OpenFlow was developed in the first place.  Its inception began a few years back at Stanford as several super smart engineers were going for their PhD.  The team seems to have been lead by Martin Casado and Nick McKewon of the Clean Slate Program at Stanford. There were also folks from other Universities, such as UC Berkeley, working on the projects as well.  The net of it is, OpenFlow was the outcome of their work.  One project they worked on was called Ethane, a way to enable “global policy” throughout a network system, and another was a more broadly defined vision, which was to have ways to develop and test new Internet protocols on large scale networks, neither of which were possible in present day networks.   I’d suggest reaching out to the Clean Slate Program if you want more history on the protocol.  More importantly though, the focus should be on the current and future state of the protocol.  

Earlier, I stated we won’t care where it came from in 2-3 years and that’s simply because, it will soon be driven by the applications that are enabled by OpenFlow or an OpenFlow-like architecture.  We won’t care about the “cause,” but rather the “effects” or “applications” it will enable.  For example, in all wireless designs now, no customer is really saying “I want a CAPWAP network,” but rather “I need to have advanced location services, security, and advanced management capabilities,” which in turn is capable through using a protocol like CAPWAP between a wireless controller and wireless AP.

### It’s about the outcome, not the protocol…

This actually leads me to my next point in that you do see many of the members of the Clean Slate program, as well as guys from Nicira and Big Switch, two start ups in stealth mode developing OpenFlow based controllers, stating they don’t care if it’s OpenFlow or another protocol being used between the controller and the switch; the overall functionality and applications that are layered on top are what is really important.  I totally agree.  Again, LWAPP was originally Cisco proprietary in WLANs, inherited from Airespace, used between APs and Controllers and that slowly changed once the standards bodies got involved, and then CAPWAP was developed and ratified as an open standard.  So, if OpenFlow morphs into something bigger and bad-er, then great!  That will just mean more advanced applications that can be enabled by the use of “controller defined networks.”

### OpenFlow is a protocol, not an architecture (anymore)!

Before we go any further, I want to clear up one point because this did change slightly over the past year.  Over the past few months (maybe last year) you would have found articles that stated OpenFlow was a protocol specification as well as an “architecture.”  Right now, it is just a protocol.  It is no longer being referred to as an architecture by name alone; someone a few months ago coined the phrase “software defined networking” that encompasses the overall architecture of a distributed control plane and data plane, by the use of some means between network nodes and a controller.  However, the only means possible or being focused on is OpenFlow, but remember, I did say Aruba has network switches enabled by CAPWAP…this is mentioned nowhere in any article I’ve read in the past 18 months.  As far as the person who coined the term SDN, I did read it online or hear it on YouTube, I just can't remember right now.  If you know, feel free to comment.  Anyway, let's keep going.

Post Update: The term update seems to be have coined by Kate Green in 2009.  See comment from Christian below.  Thanks.

### Three-Tier or Four-Tier SDN Architectures

“Controller defined networks” or now “software defined networks” are described as being a three-tier architecture with network switches, a controller(s), and applications layered on top.  When I sell/design WLAN environments, I also describe them as being a three-tier architecture, i.e. Access Points, Controllers, and Unified Management (WCS in the Cisco world).  See the difference in how the 3rd tier differs – applications vs. Unified Management.  But now that I think of it, there are also WLAN applications layered on top of all that, so either the WLAN is a four-tier or the SDN needs to re-market their pitch. 

In order to keep things simple (for us simple minded network architects), the three-tier model needs to be switches, controllers, and Unified management.  The unified management will obviously be a manager of managers and a way to easily manage an environment when there is more than one controller deployed.  Yes, this manager of managers is just an “application,” but it is a REQURIED application and should be part of an offering from day 1.  This is why we should have network switches, controllers, unified management, and then applications layered on top!

### Controllers are not a single point of failure

Quick point on controllers.  There are questions and statements such that “the controller is a single point of failure for a SDN.”  Yeah, that’s true if you’re cheap and don’t design a highly available network.  You can do that for WLAN as well, although it’s never recommended.  I really hope networking engineers are confident enough in these genius developers in that they will develop a way for High Availability (Active/Standby or Active/Active) in these OpenFlow-enabled controllers. 

### So who’s involved with OpenFlow/SDN right now? 

Well, over the past 6-12 months, there have been more and more announcements.  It's getting increasingly hard to keep up.  Nicira, still a stealth mode start mode, seems to be heavily focused on the integrations with open vswitch in large scale-out data centers.  There was a nice [announcement][1] with them and NTT a few weeks ago, but still didn’t go into much detail.  Big Switch Networks, also in stealth mode, [entered private beta just a few months ago][2].  Not sure where their exact focus is, but they may be taking a more Enterprise-driven approach to get out of the gates.  Indiana University seems to be pretty advanced with their level of network research and based on what I’ve read over the past few months, they’ve tested quite extensively with several different types of controllers and OpenFlow enabled switches.  From a large manufacturer standpoint, Brocade, HP, Dell, IBM, Arista, and Cisco have all made some type of announcement over the last several months.  The most recent [announcement][3] came from IBM, who just released a high density 10G rack mount switch that supports OpenFlow.  Cisco previously [announced][4] that its Nexus line of switches will support OpenFlow in the future – pretty cool stuff.  Both Dell and Force10 are part of the [Open Networking Foundation][5] (ONF), whose goal is to accelerate the delivery of Software Defined Networks, so since Dell’s acquisition of F10, one must think they are also working on something.  Arista’s EOS seems to be built from the ground up ready to integrate via all sorts of APIs, so it shouldn’t be too much trouble for them either to enable a protocol such as OpenFlow into their switch portfolio.  Note: all vendors [mentioned][6] except Arista are part of the ONF, not just D/F10.

[1]: http://www.ntt.co.jp/news2011/1108e/110802a.html
[2]: http://www.bigswitch.com/wp/big-switch-networks%E2%80%99-openflow-based-controller-goes-into-beta/
[3]: http://www.openflow.org/wp/2011/11/10gigabit-switch-from-ibm/
[4]: http://blogs.cisco.com/datacenter/whats-new-with-cisco-and-openflow/
[5]: https://www.opennetworking.org/
[6]: https://www.opennetworking.org/membership


### OpenFlow Deployment Models by examining the history of WLAN

When it comes to software defined networks based on the OpenFlow protocol, there are different models of deployment.  For example, the switches can support native OpenFlow only or they can support a hybrid approach where the switch still runs traditional L2/L3 protocols and a few VLANs or ports are OpenFlow-enabled.  I’m not going to dive into these, but Ivan Pepelnjak, does a good job of that in his blog [here](http://blog.ioshints.info/2011/11/openflow-deployment-models.html).  Instead, I just want to go back to the world of wireless networking to describe what is capable because the environments are so similar, and I truly believe the analogies between SDN (based on OF) and Enterprise WLAN are not being made enough.  Here are some analogies and general ideas to remember:

* WLAN began as each AP having its own full blown operation system, i.e. IOS.  These environments were difficult to manage as the number of APs increased in large enterprises
* A start up called Airespace came onto scene in the early 2000s that had a unified approach to wireless networking – APs no longer had a full blown OS on them, but a stripped down version.  In this model, APs became “thin,” “lightweight,” or “dumb” in daily conversations and “intelligence” now resided on a controller
* Cisco acquires Airespace in early 2005 because they see the value in this technology, understands the benefits and ease of management that the solution provides, and probably realized they were losing more and more deals!
* Cisco still offers IOS based APs that don’t require controllers and they also offer “lightweight” APs that require controllers
* In the lightweight model, all traffic including control and data plane traffic, was tunneled back to a controller.  This wasn’t ideal for ALL customers – some customers needed to keep certain types of traffic locally and leverage the L2/L3 on that switch the AP was connected to
* Remote Edge Access Point (REAP) was developed and allowed for at least one SSID/WLAN to be kept “local” on the switch*, while the rest would still be tunneled back to the controller
* This meant now that only control traffic was tunneled back to the controller and data traffic was terminated local by the switch for REAP enabled SSIDs
* Hybrid REAP (HREAP) was developed to enhance REAP and allow up to 16 SSIDs/WLANs to be kept local so that the customer had more flexibility in terms of deployment options.
* Continued throughout and still today, applications are being layered on top of WLANs

The point is that the OpenFlow protocol and Software Defined Networking paradigm may seem new, but there are many similarities to the trends we’ve seen in wireless LAN networking over the past 7 years.  So in my opinion, it’s really not all that new.   As WLAN is still developing, we can’t expect OF/SDN to be fully evolved by next year or the year after; it will be an ongoing process and as more functions and applications are developed into the architecture, I do believe the adoption will accelerate for SDNs.  Hopefully, for the sake of unified management, we’ll see a merge of OpenFlow and CAPWAP so that LANs and WLANs can be managed as one.  And on a positive note, I do believe the start ups Big Switch and Nicira see the relevance to the WLAN industry considering Big Switch co-founder, Kyle Forster, is a former Product Manager for the Cisco WLAN BU, and newly announced VP of Marketing for Nicira, Alan Cohen, is a former VP at Cisco who actually went to Cisco via the Airespace acquisition back in 2005.

### Calculating and Distributing State

Let’s take a step back for a quick second.  I said before when deploying a controller in an OF-enabled network, the control plane will be split from the data plane by means of OpenFlow.   This is important in understanding that if we did have a completely enabled network with OpenFlow-only switches (that weren’t forwarding standard L2/L3), networking as we know it could be gone and changed forever.  This means through the OF protocol spec, the controller will compute the state of the network based on any higher layer integrated applications or maybe just by using OSPF, or other well known protocols, and then distribute the state via OpenFlow and directly manipulate the FIB tables on the network switches.  Remember, in that example, OSPF would be run, but only on the controller.  This goes back to the OpenFlow beginnings, and how easy it would be to develop new protocols should the need arise.  What if you need a new link state protocol or just any new protocol?  In the past, one would need to develop a complex distributed system that would be run on each router/switch and have shared updates between them!  I’m not a developer, but that just sounds messy when trying to develop a new algorithm or program of any type.  With this centralized model of computing state, one could focus their time and energy at the focal point of the network and quickly deploy new protocols and applications.

[Related article](http://packetpushers.net/big-switch-network-openflow-and-virtual-networking/) and totally worth a read.  @cloudtoad goes into the details of OpenFlow and hardware designs of FIB tables, looking at where OpenFlow sits, and if OpenFlow has the separation required for network virtualization.

### What Enterprise applications would SDN work the best for?

This is a question that also comes up a lot from the small community of OF-related bloggers out there.  Personally, if I were an end user, I think the ease of network management alone would make me take a meeting with any of the vendors mentioned above.  What killer app did Airepace have in 2003-2004? My guess is it was around simplified management and deployment models in medium to large scale wireless networks.  Why would that be different for an OpenFlow based SDN?  The two others that really interest me are security (NAC-like solution) and policy based routing.  Ivan also covers these in a recent [post](http://blog.ioshints.info/2011/11/openflow-enterprise-use-cases.html) of his. For large scale out data centers, there are benefits around multi-tenancy and having the ability to create "mini" virtual switches that span several physical and logical switches as well.  Pretty sweet.

Remember in a centralized controller based model, the state of the network would be known by the controller.  This means integrating any 3rd party applications into the network would be made possible.  You could be making forwarding decisions based on just about anything you can think of.  

Be creative, think outside of the box, and chances are what you think of can be accomplished through software defined networking.

