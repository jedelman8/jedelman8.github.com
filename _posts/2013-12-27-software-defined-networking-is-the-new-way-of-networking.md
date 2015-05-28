---
layout: post
title: Software Defined Networking is the New Way of Networking
tags: sdn
---

Software Defined Networking (SDN) is the new way of networking.  It’s plain and simple.  And one of these days we’ll just go back to calling it networking because at its root, the network will still be forwarding the data needed for businesses to operate and thrive.  In this post, we’ll look at several new products and companies that have emerged over the last few years within the SDN Ecosystem and see why SDN is already the new norm in networking.

#### NEC ProgrammableFlow
Requires an OpenFlow controller that is used to communicate with physical and virtual switches.

#### VMware NSX
Requires a controller cluster.  Single point of control for a scale out virtual networking solution.  Communicates with VMware vSwitches, Open vSwitch, and 3rd party physical gateways.

#### PLUMgrid
Requires the equivalent of a controller of which they call the PLUMgrid Director to communicate with the IO Visor (their version of a virtual switch).  They also have their own SDK which would be a value add if customers are looking at building their own applications to integrate to the PLUMgrid platform.

#### Nuage Networks
Requires a controller that communicates with Open vSwitch, Nuage physical gateways, and 3rd party physical gateways.

#### Juniper Contrail
Like the previous 3 listed, Contrail is a network virtualization platform that also requires a controller to be used as part of the solution. 

#### Plexxi
Requires a controller, but also offers advanced analytics.  Their solution is a tightly integrated hardware + software physical networking platform.  

#### Cisco Application Centric Infrastructure (ACI)
Requires a controller called the Application Policy Infrastructure Controller (APIC) to manage and instantiate policy across a bed of Nexus 9000 series switches.  This means no more CLI when ACI is deployed.  Call it SDN or ACI, it’s still a new way of networking that requires a controller.

#### Stateless Networks
Offering new ways to automate and program the network via their Network Director platform.  While it’s not a typical controller in the general sense of SDN, it very well could be.  They are also developing their own software agent that can be loaded on open networking hardware and software.  The director plus the agent could be looked at as the equivalent of Puppet or Chef, but purpose built for networking.

#### Arista EOS and Nexus 9000 enhanced NX-OS
Linux Networking Operating Systems – networking vendors are opening up their typically closed up operating systems and giving customers direct access to the Linux kernel.  While Arista has been doing this for years, Cisco has recently announced this on their new Nexus 9000 line (when running in standalone mode).  Word is the Nexus 9000 will even offer access to the Broadcom shell as well.

#### HP
Shipping dozens of switches that support OpenFlow.  Haven’t checked the latest on if their controller or SDN apps are shipping just yet, but the few that I’ve seen are slick and non-disruptive to Enterprise Campus environments.

#### Cumulus Networks
Offering a Linux distribution purpose built for open networking hardware.  Cumulus does not offer a standard industry CLI.  Linux and 3rd party agents are required for management.  Customers would manage the Cumulus infrastructure with home grown scripts and tools, Chef, Puppet, CFEngine, Stateless Networks, etc.

#### Pica8
Focused on delivering a packaged solution selling commodity hardware plus software that runs on top ranging from traditional L2/L3 stacks to OpenFlow agents.

#### Big Switch
Requires an OpenFlow controller that is used to communicate with physical and virtual switches to create their Data Center Cloud Fabric.

#### Embrane
Offers a way to rapidly deploy NFV-based Layer 3-7 services.  This requires what looks and feels like a controller for advanced networking services – Embrane calls this their Elastic Services Manager (ESM).

#### vArmour
Security focused player in the SDN space leveraging an OpenFlow controller and network agents (enforcement points).

#### onePK
Cisco’s software development kit (SDK) that can be used to directly program to network devices to extract state and device information, but can also make changes that affect general configuration routing information.

#### Juniper’s XML APIs
These seem pretty robust in that each device (no controller required) support native XML APIs to be able read state/information, but also make configuration changes

#### Python
Network devices are now shipping and exposing Python interpreters via the CLI.  Scripts or agents can be loaded across device to enhance overall programmability. 

#### RESTful APIs
Almost every controller mentioned and shipping today support RESTful APIs.  The same APIs exposed should be the same APIs the vendor uses for their own CLI or UI.  This means you should be able to make any change or get any piece of information from the system directly from an API call should that be desired. 

#### OpenDaylight
Open source SDN controller looking to make its first official code sometime in early 2014.  Supporting a variety of APIs including OpenFlow and OVSDB on the southbound side, but also full REST APIs on the northbound side.

#### Open vSwitch
Open source virtual switching platform initially led and built largely by the Nicira team.  Primary protocols used within OVS for control and management are OpenFlow and OVSDB.

## The number one takeaway here is networking has already changed.  

Here are a few other trends that we can see happening:

#### Controllers are a new form of norm.
It’s kind of scary we’ve made it this far without [single points of management](/home/the-importance-of-the-network-controller) and control in the network industry.  Said differently, networks will be true systems and be operated as such instead of a collection of devices operators do their best at managing.   If there isn’t a controller, or you are choosing not to use one that happens to be optional, the switches are likely getting integrated into an environment that will have other types of automation and tools to accomplish what the controller provides, and in that case, open and programmable hardware becomes of the utmost importance.

#### The conversation isn’t about speeds and feeds.
This will be further accentuated when ACI launches next year simply because of the massive reach Cisco and their channel has throughout the industry.

#### SDN isn’t just about separating the control plane from the data plane
although some companies are still going all in on OpenFlow such as Big Switch and NEC.  It’s a detail in the solution and not a good thing, nor a bad thing, either way.

#### Programmability, Manageability, and Point of Control are critical parts of each solution
potentially more important important than speeds, feeds, and nerd knobs.  This includes vendor SDKs, device level programmability, controller extensibility, and how each component/solution integrates to 3rd party systems.  

#### Whitebox networking is real 
and being led by vendors such as Cumulus, Pica8, and Big Switch with the price points of hardware coming down so much (have you seen the price of the Nexus 9000?), it’ll be interesting to see how much traction whitebox gets over the next 18 months.

#### Network Virtualization, or 100% software based overlay solutions, are real, and also here to stay
Vendors like VMware, Nuage, Juniper, and PLUMgrid are leading the charge in this space and offering a way to fully de-couple networking from physical hardware.  

#### Data Center is the primary focus on SDN today
from whitebox, network virtualization, turnkey network systems with controllers, etc. with the primary drivers being cloud, the need for agility, and simplified operations for control, manageability and integration to 3rd party systems and tools.

#### Open Source is playing a greater role in the network community
With Open vSwitch, Open Daylight, and OpenStack Neutron, it is unclear where it’ll go, but they all seem to have legs and it’ll be quite possible to have a fully open source network running atop a commodity physical network when all said and done with.

#### There is more than one type of software only networking solution
ranging from virtual network appliances, virtual switches, software controllers, orchestration platforms, software agents, OpenFlow agents, and full blown operating systems that can be loaded onto 3rd party hardware.  

Based on product launches and the hype of the previous few years, it would seem we are going to continue to learn much more over the next 18 months.  We should expect to see and hear about real production deployments of many of these products within the next 6-9 months at a greater size and scale than is currently going on – hopefully with customers talking about their experiences as well.  By mid-2015, we should be able to see what the success of each company will be and who eats up the many startups still out there.  Of course, this is purely speculative and only my opinion.


See you all next year!

Thanks,
Jason

Twitter: @jedelman8