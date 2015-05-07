---
layout: post
title: Big Switch, Cumulus, and OpenFlow
tags: sdn, openflow, big switch, cumulus
---

Two of the three companies promoting white box, now more commonly known as bare metal, switching are Cumulus and Big Switch Networks.  There has been coverage on each of these companies, but the question always arises, “does Cumulus support OpenFlow?”  I had the chance to talk to JR Rivers, Cumulus CEO, at the last [Open Networking User Group (ONUG)](http://opennetworkingusergroup.com/) during a [Tech Field Day video](http://www.youtube.com/watch?v=acZ4n3srzd8) and heard the answer from him then, but hadn’t seen anything documented publicly. 

There was a SDN Meetup at Stanford last week where JR gave his take on SDN and a great overview on Cumulus, which happens to be on [Vimeo](http://vimeo.com/87216036).  More importantly, he touches upon the question regarding OpenFlow support in the Cumulus Linux software stack during the video.

Coming directly from JR, his response (around the 58 minute mark):

> “The only way you can truly be successful in meeting the customer needs around OpenFlow is to be truly focused on a great OpenFlow agent that lives on the switch platform.   Trying to come up with a hybrid approach or half approach inevitably end up in unhappy customers… In general, when customers want to use OpenFlow, Cumulus will say, go talk to Big Switch.”  - JR Rivers

On the flipside, this is a similar answer Big Switch Networks gave last year during [Network Field Day 6](http://vimeo.com/80199772) (watch the first 15 minutes) when asked if they would support L2/L3 and if their OpenFlow agent would run on Cumulus Linux.

### Big Switch / Cumulus Summary:  

**Cumulus offers Cumulus Linux** –  a native Linux distribution based on Debian that offers a traditional L2/L3 stack for bare metal switches.  No OpenFlow.

**Big Switch offers Switch Light** –a combination of Ubuntu plus an OpenFlow Agent (Indigo) for bare metal switches.  No L2/L3 stack.

These guys are competing for the same budget, but clearly have different technologies.  But since you may find the same switches on their hardware compatibility lists, you could deploy Cumulus Linux today, love it or hate it, and then change out the software being used to Big Switch’s Switch Light (or vice versa) and have a fully different network and operational model, while not touching or swapping out any piece of hardware.  Think about that.

Going back to the JR video, you will notice as JR answered the question around OF support, he pointed at experiences he had while working at Cisco in the early days of OpenFlow.  If what he states is still valid, where does that leave companies trying to develop hybrid solutions like Brocade and HP?  Where does that leave [Pica8](http://www.pica8.com/open-technology/open-vswitch.php) that happens to offer two modes of operation on their hardware, one that runs OVS and one that runs a standard L2/L3 (and each mode of operation runs over the same Linux OS).  

*Who’s right?  Who’s wrong?  Or at what scale/performance, does it matter?*

### Some General Thoughts

* Watch both videos  – hearing this kind of candidness from JR Rivers and Rob  Sherwood is interesting to say the least
* JR talks about the de-coupling of software and hardware as a necessity for SDN.  While I think de-coupling is inevitable, calling it SDN can be a good debate.  Do we call servers software defined servers?
* If I had a shot of liquor every time JR said “Fly, Be Free” in this video, I would have passed out half way through.  It’s a great line and really does sum up where the industry is going.  #flybefree
* Diane Greene was co-founder/CEO of VMware.  She was an early investor in Nicira, but also noteworthy, an investor in Cumulus.  While one could argue that for a true SDDC, VMware would need software stacks in the network overlay and network underlay.  Acquiring Cumulus, would give VMware  just that.  For anyone who things there is a battle between Cisco and VMware now, imagine if VMware did buy Cumulus?
* Regardless if Cumulus does get acquired, there is still a need for other independent network operating systems, so customers do have choice.
* Towards the end of the video, there is also a question on in if Cumulus Linux will have other distributions or will run in other parts of the network outside of the data center (WAN, etc.) and even support Fibre Channel.  I’ll let you watch and see how JR answers.

Thanks (and Fly, Be Free),
Jason

Twitter: @jedelman8