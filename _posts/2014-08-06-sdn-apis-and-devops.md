---
layout: post
title: SDN, APIs, and DevOps
tags: sdn, devops
---

There was a recent blog by Mark Burgess, founder and creator of CFEngine. It is a must read (on his personal blog).  He really makes you think where we are as an industry, question if we are on the right path, and quite frankly calling out certain technologies as pity attempts compared to what is needed. Regardless of all that, we cannot forget one key point, the industry is in fact moving forward right now. 

As I read his post, I remembered a conversation that I saw on twitter not too long between John Willis, Lori MacVittie, and Joe Onisick. It was more or less on the intersection of SDN and DevOps. 

The article by Burgess and the Twitter conversation really got me thinking. 

When you combine these interactions, even at the highest levels, you have to wonder, what is the right approach from both a vendor (product) and user standpoint? Are we on the right path? Do we have it all wrong? Must we fail first before getting it right?

### SDN Will Simplify

SDN controllers will totally simplify things, but even then, the APIs they expose are arguably too low level for the average consumer of the SDN solutions. Who will actually use these APIs? Who uses vCenter APIs? Where does this leave admins, engineers, and automation tools? 

In an SDN solution, there will probably be very little API integration by the end user. But again, are the APIs being exposed too level? Who will even use the APIs? 

It's two main camps. It's high end users and other technology companies be it for automation tools or 3rd party frameworks and other types of infras integration, mgmt, tooling, etc.

### Does DevOps still have a place in an SDN World? 

If an SDN solution is deployed, how will data be gathered, interpreted, and correlated? How will ad-hoc or network wide changes be made? Will it be through 10s or 100s of clicks in the slickest UI we have yet to experience in the network industry? Maybe for some, even most, but does it have to be?

### Enter NetOps

This is why DevOps tooling for networking, dubbed NetOps, is still extremely important. While the focus thus far has been on controllers here, the same questions thought process can be raised for device level APIs. 

NetOps/DevOps tooling, in my personal opinion, can give a wider audience what is needed. This isn't access to APIs, it's access to their infrastructure, it's access to their data. The data that is currently hidden in every fabric and device out there. 

Using Ansible as an example, imagine this- a policy based or application centric module? Maybe an application centric playbook.  This means the users do NOT deal with the low level APIs. They use modules that take the API and the "how" off the table. They make it about the "what" or the intended "policy." And once these "whats" have been developed, applications can be built on top for even further value add.

I do apologize for the formatting and not linking to the article and conversation mentioned, but I just wrote this while boarding for a flight.  They shouldn't be too hard to find. Will update as soon as I can.

Thanks,
Jason 

Twitter: @jedelman8