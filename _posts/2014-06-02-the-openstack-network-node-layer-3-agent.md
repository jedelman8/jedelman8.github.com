---
layout: post
title: The OpenStack Network Node - Layer 3 Agent
tags: openstack, neutron
---

When networks are deployed in a box by box model, network admins know exactly what, where, and how something is being configured.  In highly dynamic environments, this may not be the case.  This is why it’s crucial to understand what is really going on behind the scenes.  In OpenStack, there are several components that together are comprised to make OpenStack Networking (aka Neutron).  These include the Neutron server, dhcp agent, metadata agent, L3 agent, and then the agents that would reside in the infrastructure to be programmed (on either physical and/or virtual switches).  For example, in Open vSwitch deployments, there would be a Neutron OVS agent on each host/server.  And this could vary based on which particular vendor plugin is being used too!
In this post, I’m going to mainly focus on the Neutron Layer 3 agent because I had a hard time grasping this one at first.  It turns out that it’s not so bad after all.

When I first started reading about Neutron, I saw many references that there was only one (1) layer 3 agent supported in a given deployment.  That just didn’t seem to make sense because that would surely be a massive chokepoint.  This was the case, but fast forward to the last few releases and multiple layer 3 agents are supported. 

### Let’s dive in…

First, we need to understand a layer 3 gateway is required to communicate to the outside world, i.e. the non-OpenStack environment.  Sometimes this is referred to as the external network, provider network, or Internet.  It really varies based on who you’re talking to and if they have any type of network background.

For the rest of this post, I’m going to change it up and go with a Q&A style post to answer a few questions regarding the L3 agent/gateway and the ‘neutron-server’ that helped me understand this a bit better.  Hope it can do the same for others too.

Sample diagram below showing a network node with L3 agent installed.  With three tenants, there would be three virtual routers on the network node.

![nnode](/img/node1.png)

#### What’s the different between a layer 3 gateway and a logical (or virtual) router? 

This may be where I was stuck initially, but you can take a beefed up Linux machine and call that the Layer 3 gateway.  So, this would be the typical “physical” perimeter router for North/South traffic.  Within, or inside, the physical server/router, there will be logical/virtual routers.  Said a little differently, there will be logical routers instantiated on the layer 3 gateway node.  Going a step further, each logical router is deployed by using Linux Network Namespaces.  The namespaces are analogous to VRFs in the network world. 

**Note:** the layer 3 gateway could be a physical or virtual machine.  However, it is common to use a physical machine due to potential high network I/O requirements.  But even more common to have all OpenStack services on one or two machines in test/dev OpenStack enviornments!

**Tangent:** I think my initial struggle to understand this is why would you only have one network node in a multi-tenant environment.  My instinct was to have one (1) virtual router per tenant using virtual machines to offer a scale out, guaranteed performance, network perimeter per tenant.  Turns out this is technically possible, but many OpenStack environments that need to operate at scale would then need to burn up an enormous amount of compute for gateway services.  This would be analogous to NOT using VRFs in the traditional network world.

#### Okay, that’s layer 3 gateways and logical routers, but what is a gateway service?

A gateway service is one step higher in the level of abstractions being used.  I can best describe this using VMware NSX.  NSX supports up to three (3) layer 3 gateway services.  Each gateway service supports up to ten (10) layer 3 gateways.  Each gateway could support 10s-100s or more (based on capacity) logical routers.  Don’t forget each logical router is a Linux namespace.  The tenant’s virtual router is deployed onto a gateway service which in turn picks a gateway to deploy it on.  The scheduling and the selecting of which gateway to use is still primitive and seems there are things coming in the next release OpenStack/Neutron release to improve this.

**Note:** OpenStack documents state Neutron itself supports multiple network nodes (layer 3 agents), but I couldn’t find any specifics to compare to the numbers I used for NSX above.  If anyone has these numbers, please feel free to comment below.


**Update:** While OpenStack Neutron itself supports multiple Layer 3 gateways, the "gateway service" is a VMware NSX specific construct (not Neutron).

#### Where does the default gateway reside for hosts in a given logical network segment? 

As you may guess, this would be the “inside” interface of the logical router, or network namespace.  The namespace would connect to at least two Open vSwitch bridges within the network node, one that connects to the inside OpenStack environment and one that connects to the “outside world.”

#### What if a single tenant needs multiple layer 2 segments? 

Multiple logical segments can be attached to a logical router.  In the backend, this just means, the namespace will have more than two interfaces. 

#### If a logical router can have multiple internal interfaces and route between them, does this mean all Layer 3 traffic between those network segments need to be hair-pinned all the way back to the network node where the L3 agent resides?

Interestingly enough, this was the case, and may still be the case based on a few factors.  But, it is in fact possible to leverage the layer 3 daemon (ovs-l3d) in Open vSwitch to enable and take advantage of distributed layer 3 routing in the kernel.  In this deployment, each local instance (per host) of OVS will use the MAC addresses of the logical router (namespace) interfaces on the gateway node to enable in-kernel routing on each host for all East/West traffic that share a common logical router.  Only traffic destined to the outside world would then need to transit the network node (layer 3 agent).

**Note:**  if NAT is a requirement, traffic would also need to transit the network node.

#### Does the layer 3 agent need to be on a dedicated “network node?”

No.  As can be seen by many development/test OpenStack distributions, the layer 3 agent is often found on the “OpenStack Controller” where the neutron-server would also reside.  These test stacks also make it more confusing to understand all of the components because they all reside on the same machine.

#### Does “OpenStack Controller” have anything to do with an “SDN controller?”

No, no, no.  From a network/neutron perspective, the OpenStack controller is where the ‘neutron-server’ service would reside and basically expose the Neutron APIs for other OpenStack services to consume.  As defined by OpenStack, “neutron-server provides a webserver that exposes the Neutron API, and passes all webservice calls to the Neutron plugin for processing.”  This means that in a SDN or Network Virtualization environment that happens to use a SDN controller, the neutron-server would then be passing the API calls to the SDN controller, i.e. OpenDaylight, NSX Controllers, Cisco APIC, etc.

These technologies are still emerging and if you happen to be someone deeply immersed into OpenStack Neutron and feel I may need make any corrections, please don’t hesitate to leave a comment below.


Thanks,
Jason

Twitter: @jedelman8

