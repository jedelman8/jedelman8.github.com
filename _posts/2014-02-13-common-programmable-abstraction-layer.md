---
layout: post
title: Common Programmable Abstraction Layer
tags: cpal
---

In late January, there were some big names on stage at the latest [Open Compute Summit](http://www.opencompute.org/blog/ocp-summit-v-the-future-is-open/).  I’d like to focus on one keynote panel that was called, “Opening Up Network Hardware.”  The panelists for this session included Martin Casado (VMware), Matthew Liste (Goldman Sachs), Dave Maltz (Microsoft), and JR Rivers (Cumulus) and was led by Najam Ahmad (Facebook).  If you haven’t watched the session already, it’s definitely worth it.  You can check it out [here](http://www.youtube.com/watch?v=fzA4VIUnAIw).

It’s always interesting when there are consumers of the technology on stage with those that sell and build their own technology.  Even in a session like this, it’s hard to put Facebook and Microsoft in the consumer/user group because they are increasingly rolling their own.  But Goldman Sachs (GS) on the other hand is unique --- and more relevant to the Enterprise.  They are a user of Enterprise technology, but what makes them unique, in my opinion, is that they seem to be crossing the chasm or straddling Enterprise technology and rolling their own (DIY) with technology from [OCP](http://www.enterprisetech.com/2014/01/29/goldman-sachs-fidelity-bank-open-compute/) and what they [announced](/home/goldman-sachs-is-deploying-sdn-are-you) at last year’s ONS, which was soon after they [joined the ONF](http://www.sdncentral.com/news/sdn-heats-up-goldman-sachs-joins-the-open-networking-foundation-board/2012/11/).  And there is probably more than that going on.  Maybe we’ll hear more at this year’s ONS too.

In the panel mentioned above, [Matthew Liste](http://www.linkedin.com/pub/matthew-liste/0/209/832), talks about a “common abstraction layer” that enables you to consistently execute services like creating a VLAN across different hardware types (it’s around the 14:10 mark in the video).  JR actually counters and says, “you don’t need that normalization you are talking about” if you can load a consistent network operating system on your choice of hardware.  While JR makes a valid point and it is, of course, true, remember he is the CEO of Cumulus, and is always selling :). 

More importantly, we have to remember, Liste’s point is more than valid and will always be valid.  There are industry transitions, company transitions, acquisitions, and changes that can happen in technology consumption.  Even if you adopt Cumulus 100% today, having a common abstraction layer, still makes sense.

It’s not new to talk about common, or standard, APIs.  With the latest industry buzz around SDN controllers though, most of the chatter is about standardizing controller northbound APIs (NB-APIs).  Even as I talked about the [importance of the controller](/home/the-importance-of-the-network-controller), the industry has vendors like Arista and Cumulus who offer open platforms (no controllers from them directly) , don’t view their networks necessarily as a single unified system, but let the customers unify it if they wish. 

In October, Cisco announced the Nexus 9000 that can operate in one of two operational models --- one that leverages a controller (APIC) that puts applications first and another that leverages the openness of the software itself, more like the Arista and Cumulus model.  The operational models talked about by Cisco during the ACI launch should be looked at more broadly by the industry.  Both models make sense.

There won’t always be a controller, but there very well could be.

This is why I believe, as an industry, we should look at exploring a common programmable abstraction layer for network devices.  The same abstraction layer should be exposed and/or integrated with controllers.

In the next post, I’ll give an overview of some work I’ve been doing in this area that was motivated by Jeremy Schulman, who is aspiring to bring DevOps to networking.  This will help convey what I’m referring to with a **“common programmable abstraction layer” (CPAL)**.  It also just so happens I was working on this project during OCP week, and hearing Matthew Liste mention the well needed common abstraction layer (not referencing a controller), helped motivate the actual write up.  Thanks to them both.

Stay tuned for the dirty details.

And of course, let me know what you think. Do common APIs at the device make sense?  Or should all the time and energy be spent on controller NB-APIs?

Thanks,
Jason

Twitter: @jedelman8

Follow-up posts:
[The Power of a Programmable Abstraction Layer](/home/the-power-of-a-programmable-abstraction-layer)

[Demo: Common Programmable Abstraction Layer](/home/demo-common-programmable-abstraction-layer)
