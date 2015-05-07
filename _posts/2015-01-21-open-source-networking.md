---
layout: post
title: Open Source Networking
tags: open source
---

We’ve heard a lot of Software Defined Networking (SDN), Open Networking, APIs, and policy models over the past few months (and years).  There are days where it’s sickening to hear the term SDN, but even on those darkest days, the reality is that the network industry has a bright and open future.  In this post, I’m going to share a list of networking projects that I’m aware of that are not only open, but also open source.  It is definitely eye opening and extremely positive to see so much open source activity in the network industry.

![Stallone-Daylight](/img/stallone-daylight.jpg)

Edit/Note: updated list can be found here on [GitHub](https://github.com/jedelman8/open-source-networking/blob/master/README.md).  Feel free to issue a pull request to add or modify the list.

[OpenDaylight (ODL)][1] – established in April 2013 is an open source Software Defined Networking (SDN) controller platform(s).  There are different controller platforms for different use cases.

[1]: http://www.opendaylight.org/

[OpenFlow (OF)][2] – established in the late 2000s, the OpenFlow 1.0 release launched in December 2009.  The Open Networking Foundation took over the development (not actually coding) of OpenFlow when ONF formed in late March / early April in 2010.

[2]: https://www.opennetworking.org/ja/sdn-resources-ja/onf-specifications/openflow

[Open vSwitch (OVS)][3] – established in mid to late 2009 by the Nicira team to replace the standard Linux bridge.  It’s goal was to provide full featured switching capabilities to virtual environments running in Linux based hypervisors such as KVM and XEN.  It’s also now being used to inter-connect containers.

[3]: http://openvswitch.org/

[Open Virtual Network (OVN)][4] – just established last week and will be a sub project of the OVS project.  Its goal is to provide native abstractions to OVS to simplify the provisioning of logical segments, logical routers, and security groups without requiring a SDN controller (initially for OpenStack environments).

[4]: http://openvswitch.org/pipermail/dev/2015-January/050380.html

[OpenStack Neutron][5] – formerly known as Quantum, it is the core networking project and API for the OpenStack project.  For networks to be orchestrated by OpenStack, they must have a neutron plug in.  It officially became a core project in the OpenStack Folsom release.

[5]: https://wiki.openstack.org/wiki/Neutron

[OpFlex][6] – launched in early 2014 by Cisco as a way to create, deploy, and manage policies. It’s initially being used as part of the Cisco Application Centric Infrastructure (ACI) solution.  Cisco has also submitted it to the IETF and is contributing work around OpFlex to the ODL and OpenStack Neutron communities.  The OpFlex project consists of three things: the OpFlex protocol, SB plugin, and the policy agent.

[6]: https://tools.ietf.org/html/draft-smith-opflex-00

[Data Services Engine (DSE)][7] – initially created in 2013 by Plexxi, the DSE has become part of the OpenStack Congress project (focused on policy).  The DSE could revolutionize how different products and solutions communicate with one another.  I’ll go out on a limb and say it could be the foundation for machine learning in the data center (and IT, more generally).

[7]: https://github.com/stackforge/congress/tree/d1ef962a7e6e1a55537d50ebb3604ade73ba2588/congress/dse

[Quagga][8] – this one has been around for some time, but is gaining more visibility with companies like Cumulus actively promoting open networking.  Quagga is a routing software suite and has open source implementations of OSPF, RIP, BGP, etc.

[8]: http://www.nongnu.org/quagga/

[OpenContrail][9] – Juniper acquired Contrail, a network virtualization company, in December 2012.  Shortly after, they open sourced the product and called it OpenContrail.

[9]: http://www.opencontrail.org/

[MidoNet][10] – the network virtualization product by Midokura was just open sourced less than 3 months ago.  Like a few other startups and solutions out there, they have been primarily focused on networking for OpenStack clouds.

[10]: http://www.midokura.com/press-releases/midokura-open-sources-complete-iaas-network-virtualization-solution-openstack-community/

[Open Networking Install Environment (ONIE)][11] – created by Cumulus Networks in 2012 and later adopted by the Open Compute Project (OCP), it is a lightweight operating system serving as a boot loader for bare metal switches allowing users to have choice in which operating systems can be used and loaded.  Cumulus, Big Switch, OCP, Pica8, Dell, and Plexxi (they’ve committed to it, but don’t know where they stand) are now shipping switches with ONIE pre-installed and/or support ONIE in their software.  You can find plenty whitebox/bare metal platforms that ship with ONIE that will allow you to load network operating systems like Cumulus Linux, Open Networking Linux, PicOS, etc.

[11]: http://onie.opencompute.org/

[Open Compute Project (OCP)][12] – has been around since 2011 initially established by Facebook to share some of their hardware designs, but also to help accelerate hardware and software innovation in the open source world.  OCP Networking was later established in early to mid 2013, publicly launched at Interop in 2013.  It’s since been working on open source architectures for data center top of rack switches.

[12]: http://www.opencompute.org/projects/networking/

[Prescriptive Topology Manager (PTM)][13] – created by Cumulus in 2013 as a way to dynamically verify there is up to date and accurate cabling in place in switched networks.  This is great and I do hope other vendors adopt PTM.  I’ve been developing external (off box) functionality to do the same thing.  Check out my [blog](http://www.jedelman.com/home/prescriptive-topology-manager-ptm-support-with-nx-api-on-the-nexus-9000) for more details.

[13]: http://cumulusnetworks.com/blog/complex-topology-and-wiring-validation-in-data-centers/

[Open Networking Linux (ONL)][14] – emerged onto the scene in late 2013 by Big Switch Networks and is now part of the Open Compute Project (OCP).  ONL is a base level Linux distribution for bare metal switches.  One of its main goals is to be the foundation of which others/vendors can build commercial solutions on top of bare-metal switches.  Big Switch’s SwitchLight OS is an example, and is built on top of ONL.

[14]: http://opennetlinux.org/

[Bird Internet Routing Daemon (BIRD)][15] – similar to Quagga, it is an open source routing platform for Linux systems.  In contrast to Quagga, BIRD offers multiple-RIB design, is often used as a route server, and does not support ISIS.  On the other hand, Quagga is a single-RIB design, supports ISIS, and it’s not even recommended to be used as a route server.

[15]: http://bird.network.cz/

[Snort][16] – many are aware of Snort, but it’s an open source IDS/IPS; its commercial version came via SourceFire, a company which Cisco acquired in 2013.

[16]: https://www.snort.org/

There are plenty more open source projects that will directly impact networking too.  A few of them include:

[OpenFlow controllers][17] – there are plenty of these still out there, many in academia

[17]: http://yuba.stanford.edu/~casado/of-sw.html

[Git][18], [Gerrit][19], [Jenkins][20], [Travis CI][21] – open source tooling for source control, code review, testing, and continuous integration.  These aren’t too popular in the network community, but they can definitely have a positive impact.

[18]: http://git-scm.com/
[19]: http://code.google.com/p/gerrit/
[20]: http://jenkins-ci.org/
[21]: https://travis-ci.com/

[SocketPlane][22], [Weave][23], [Akanda][24] – new onto the network scene, SocketPlane and Weave are building open source networking solutions for Docker and Akanda (spin out from Dreamhost) is building an open source network virtualization platform for OpenStack environments.

[22]: http://socketplane.io/
[23]: http://weave.works/
[24]: http://www.akanda.io/

[Ansible][25], [Puppet][26], [Chef][27], [SaltStack][28], etc. – there is an increasing amount of open source automation and configuration management tools that have been around for quite some time, but we’re just starting to see what kind of impact and what the integrations will look like for the network community

[25]: http://www.ansible.com/home
[26]: http://puppetlabs.com/
[27]: https://www.chef.io/
[28]: http://www.saltstack.com/

And we can’t forget about [OpenNMS][29], [Zenoss Core][30], [Nagios Core][31], etc.  If you want to add more to this list, feel free to write in or comment below. 

[29]: http://www.opennms.org/
[30]: http://www.zenoss.org/
[31]: http://www.nagios.org/projects/

### Going Forward

As you can see there is an increasing amount of activity in the open source world as it pertains to networking.  While SDN is something that still can’t be defined, only sold to customers, it should be evident the role of open [source] networking will be significant going forward.  There are huge opportunities to break these projects and technologies down to make them more consumable for the average user (that is often forgotten about).  Because of this, I wouldn’t be surprised if we start to see different types of engagements with vendors, VARs, and integration companies.

Thanks,
Jason

Twitter: @jedelman8


