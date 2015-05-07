---
layout: post
title: CAPWAP and OpenFlow - Thinking Outside The Box
tags: openflow, sdn
---

After reading Ivan Pepelnjak’s ([ipspace](http://blog.ioshints.info/2011/11/openflow-enterprise-use-cases.html)) and Martin Casado’s ([networkheresy](http://networkheresy.wordpress.com/2011/11/17/is-openflowsdn-good-at-forwarding/)) blogs recently, I noticed they were making general comparisons on network tunneling protocols.  These protocols are nothing new, for example using UDP, GRE, EoMPLS, VPLS, and a new one being mentioned over the past several months, VXLAN.  However, what caught my attention was CAPWAP was also a protocol each of them used to compare to GRE, UDP, and VXLAN.  As you’ll recall in my recent OpenFlow post, I spent quite a bit of time comparing OpenFlow to CAPWAP in the sense OpenFlow is being used to separate the control and data planes on switches and CAPWAP is being used to separate the control and data planes on Access Points.

I figured, why not, let’s google CAPWAP and OpenFlow together and see what comes up.  No surprise, you see the post from Matt Davy at IU who was drawing a similar comparison to OF/SDN to CAPWAP/WLAN, my recent blog, Ivan’s blog, Martin’s blog, and then finally the reason why I’m writing this – I saw a link to openvswitch.org that talked about building support for CAPWAP into the open vswitch.  Interesting, right?  Well, to me it is!  So after digging further, it looks like Jesse Gross (from Nicira, the company who does much of the development work on open vswitch) had some comments in the commit log for this feature.  

Here is the [link](http://openvswitch.org/cgi-bin/gitweb.cgi?p=openflow;a=log;h=refs/tags/v1.1.0pre1) and exact writeup by Jesse:
 
>“datapath: Add support for CAPWAP UDP transport.
>   
>   Add support for the transport portion of the CAPWAP protocol as an alternative to GRE for L2 over L3 tunneling.  This is not full support for the CAPWAP protocol.  CAPWAP covers management of wireless access points and describes a control protocol for setting those devices up.  It also describes a data plane protocol that allows packets to be tunneled to a controller for inspection. This data plane protocol is the only component covered by this commit.
>   Signed-off-by: Jesse Gross”  

Btw, this was dated a while back, August 2010. 

Another interesting point here is that Nicira has only implemented the data plane protocol piece of CAPWAP to meet the requirements of tunneling between vswitches.  Why hasn't anyone else done this in broader sense of supporting it general L2/L3 devices?

This got me thinking, and the more I thought, the more questions I was asking myself, which is normally the case anyway.  So here they are:

1. Is there any other device in the industry that supports CAPWAP (full blown or data plane protocol) besides access points, WLAN controllers, and open vswitch?
2. What drove Nicira to implement CAPWAP on openvswitch?  I actually quite like it – it’s definitely thinking outside of the box because it is known as a protocol used in WLAN environments, although as we/I see now, it is just yet another tunneling protocol.
3. Wireless APs have the ability to speak CAPWAP to a first hop controller and then based on WLAN/SSID, the first hop controller can build another tunnel (EoIP) to what is the "guest" anchor/controller that is commonly deployed in the DMZ to implement path isolation and security for guests.  Wouldn't it be great if the guest anchor could be a L2/L3 device...maybe even a OVS?
4. What does OpenFlow use to tunnel packets from an edge device back to a controller in the event there isn’t an entry already in the FIB table…or is an L2 adjacency required between a controller and OF-device?
5. While only the data plane portion of CAPWAP has been integrated into openvswitch, has Nicira also built support for full CAPWAP into their controller in order to support wireless access points?
6. Could OpenFlow be used as the control protocol and CAPWAP as the data plane protocol for access point support on OF-controllers?  Not sure this would make sense, but figured I’d get all the st*p!d questions out of the way!

These are really open ended questions on my part, but for any of you folks out there that have coded these protocols or is extremely intimate with the protocol specifications, I’d love to get your take.

Thanks,
Jason 
@jedelman8

