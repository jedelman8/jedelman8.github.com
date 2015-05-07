---
layout: post
title: ONS 2014 - Looking at Programmable NFV, Google, MSFT, Embrane, and Big Switch 
tags: ons, sdn
---

It’s been two weeks since I attended my 3rd consecutive Open Networking Summit (ONS) and I’m glad to say, I finally found some time to get some notes and thoughts on paper about the conference.  Here are some on SDN at Google and Microsoft, and how they compare and contrast to industry incumbents’ solutions, but also how programmable NFV can be game changing in the Enterprise.  I also include thoughts on how Embrane and Big Switch play into this.

### Enter Andromeda

Google talked about their [home grown] network virtualization solution.  It leverages a custom SDN controller called Andromeda that controls physical switches, virtual switches, programmable NFV devices, and also ties into the storage platforms deployed.  Google talked about how they have showed industry leadership with technologies such as GFS, MapReduce, B4 WAN, etc.  If I extrapolate, they expect Andromeda to do for data center networking what GFS is doing for distributed scale out storage.  Who will be the Nutanix of networking?

Google, like few others, still define SDN as the separation of the control plane and data plane.  Google states, “logically centralized/ hierarchical control plane with peer to peer data plane beats full de-centralization.”  I tend to agree, but that’s a topic for a different conversation.

### A Microsoft Repeat…

Microsoft pretty much gave the same talk as last year.  I [wrote about it](http://www.jedelman.com/1/post/2013/09/microsoft-azure-sdn-networking-all-policy-in-software.html) several months ago although they did have some new slides.  They are doing what we largely define as software only network virtualization (think NSX) in Azure.  They call it Host SDN.  Maybe not in Azure, but Microsoft is also said to be dong [BGP SDN](http://blog.ipspace.net/2013/10/exception-routing-with-bgp-sdn-done.html) as well – this wasn't discussed at ONS though.  In contrast to Google, Microsoft deploys several types of controllers, not necessarily in a hierarchical design.

### Cisco vs. VMware?

So, we have Google and Microsoft, or the Google design vs. Microsoft design.  Google believes in physical + virtual and even integrates with storage and NFV, while Microsoft is currently focused on virtual only.  This sounds interesting – what other companies does this remind you of?  And maybe how controllers work together in a given solution can also be analyzed a bit more in these designs – ones that expose functional abstractions to consume by other controllers  and application or all of them purpose built as a system/solution in a hierarchical fashion?  Need to think about this some more.

### Programmable NFV – making magic happen

Google talked about programmable NFV, which made me think about NFV a bit different than I have in the past.  I realized there hasn’t been a huge amount of focus on this architecturally on what this means within the Enterprise.  Usually, we talk about a group of servers dedicated for network functions, which largely will do the trick.  This is why I’m long [Embrane](http://www.jedelman.com/1/post/2013/03/why-cisco-should-buy-embrane.html).  They have what is needed for a L4-7 network services controller (Embrane Elastic Services Manager (ESM)).  We also know there are L4-7 vendors out there that do not 100% embrace NFV (don’t need to name names) and there are still performance limitations (since they don’t want to embrace it) without using performance optimizers, DPDKs, etc. 

One answer here becomes programmable NFV solutions that can scale to multi-gig performance and beyond, offer millions of TPS, SSL offload, and the like.  For these solutions, a vertically integrated solution usually wins out based on FUD.  But, with companies like [Netronome](http://www.netronome.com/), this could and will change.  Think of them as providing the hardware that these L4-7 companies are already using for their own solutions and can be re-programmed to be a FW, IPS, or a load balancer on demand and as needed.  I heard one of Netronome’s biggest customers is SourceFire, who Cisco recently acquired, that has FW, IPS, and anti-malware solutions.  Imagine a world with Netronome hardware deployed in parallel to commodity servers controlled by something like Embrane’s ESM that rapidly provisions virtual network functions on the hardware best suited for the need – Netronome for a 10G single tenant FW that is only needed temporarily to QA a new application and general compute capacity for small scale FWs, LBs.  This is game changing.  Combine COTS programmable hardware, commodity servers, and SDN technology, and we get some real use cases to be proud of as an industry.

### Big Switch…

Big Switch had Rob Sherwood present.  Their messaging is becoming more concrete and I’ll actually say I am also long Big Switch, which may surprise some because they’ve had quite a bit of churn and pivoted last year moving away from an overlay strategy.  They had a long rough patch and may still be in it, but in another 12-18 months, I think they will be seeing the light.  They are embracing bare metal, physical and virtual, and I actually think if integrated with an NFV controller (or dev their own) and hardware from that of a company like Netronome, it becomes to look like the Google solution (as does Cisco).  Big Switch seemingly had problems trying to re-invent networking in a controller.  While it looks like a lot of the leg work is done and improved drastically, it will be companies like Big Switch that can rapidly introduce new features since they are de-coupled from HW, but also because they do have the control plane and data plane separation that even Google talks about.  Minor gripes with their presentation…while their messaging is more concrete, they shouldn’t need to talk about a programmable patch panel as a use case for SDN network fabrics. It’s just not needed.  And live demos --- they must stop.  Like I said, I am long Big Switch, but I think they are 0 for 3 with live demos when I’m in the room.  Maybe it’s me?

### Major takeaways:

* Pay attention to NFV.  Even more attention to programmable NFV, i.e. SDN enabled NFV.  
* Pay attention to programmable hardware.  Even more attention to programmable hardware that is not a switch.
* Pay attention to controllers who offer more than L2/L3 networking.  Even more attention to controllers who offer service chaining and L4-7 services.  You’ll get my ears even more if it’s a Data Center controller that integrates with compute, storage, AND networking.  Wait, does this sound like OpenStack, but a bit more intelligent?  
* As I write this, I realize there is still a lot of opportunity out there for new companies to offer full stack data center OS solutions that are fully integrated and tested, but horizontally coupled.  Interesting times indeed.
* And of course, keep learning, thinking, and asking questions!

Thanks,
Jason

Twitter: @jedelman8