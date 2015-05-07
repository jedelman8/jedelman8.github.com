---
layout: post
title: VMware Meets The Physical Network --- What If?
tags: startup, vmware
---

With other acquisition [rumors](https://www.sdncentral.com/news/big-switch-pluribus-verge-getting-acquired/2014/10/) floating around, I figured I would add my own 2 cents and do some speculating. It’s not uncommon to hear that VMware might acquire Cumulus.  

Like others, it’s one acquisition that I’ve speculated about for a while.  There is already an interesting dynamic between Cisco and VMware, but as both companies continue to go to market with their Software Defined Networking (SDN), or controller based solutions, VMware still needs to run over a physical data center network.  The physical network market is still largely dominated by Cisco though.  Does VMware want or need to control the physical network?

### Extending the SDDC

If VMware took their network strategy one step further and kept true to the Software Defined Data Center (SDDC), they would need a network operating system (NOS) that could run on approved hardware, e.g. hardware compatibility list (HCL).  They need a bare metal (white box) switch company.  Cumulus fits this build well because they are focused on creating open IP fabrics using tried and true protocols and already have their own [HCL](http://cumulusnetworks.com/support/linux-hardware-compatibility-list/).  They’ve also already partnered with VMware and [support VXLAN termination on certain platforms](http://cumulusnetworks.com/solutions/network-virtualization-overlays/vmware-nsx/).  Another point worth noting is that Diane Greene, VMware founder, was an investor in [Nicira](http://www.networkworld.com/article/2244319/virtualization/stealthy-start-up-backed-by-vmware-founder-diane-greene-looks-to-virtualize-networks.html) and [Cumulus](http://www.crunchbase.com/person/diane-greene).

### VMware’s new Chief Technology Strategy Officer

Just over a month ago, [Martin Casado replaced Steve Mullaney](http://ir.vmware.com/releasedetail.cfm?ReleaseID=866805) as the new SVP & GM of the Networking & Security Business Unit (NSBU).  This left an opening at VMware for a new CTO in the NSBU.  This hole was filled recently when [VMware announced it had hired co-founder and former CEO/CTO of Big Switch Networks](http://thevarguy.com/private-and-public-cloud-infocenter/100714/vmware-hires-big-switch-founder-guido-appenzeller), Guido Appenzeller, as the new Chief Technology Strategy Officer. 

With VMware hiring Appenzeller, this gives VMware intimate knowledge of Big Switch’s Big Cloud Fabric (BCF), a fabric built for physical and virtual data centers, that also has its own HCL.    [Big Cloud Fabric recently went GA](https://www.sdncentral.com/news/big-switch-networks-reboot-reaches-shipping-phase/2014/09/) and uses the OpenFlow protocol, the protocol that was initially developed by Martin Casado and incubated in the lab that Appenzeller ran, while both were at Stanford.  You can see there is a lot of history between these two and the same can be said for others that are still on the Big Switch team.  And for what it is worth, NSX-MH also supports OpenFlow...and both products support OpenStack Neutron.

### Which bare-metal switch company makes more sense for VMware?

Could VMware acquire Big Switch and offer an integrated Data Center network combining NSX + BCF using NSX for the virtual overlays and use Big Cloud Fabric (BCF) for their capability to manage physical switches within the data center?  Sounds like it could work, but Big Switch has a growing business for their Big Tap product line, which may not have a fit at VMware. 

And if VMware acquires Cumulus, VMware gets the capability to build out very large IP fabrics leveraging traditional protocols like OSPF and BGP.  The downside for this option is there is not an inherent “vCenter” or single point of management as you have already with server virtualization, NSX, or Big Cloud Fabric.  That said, if you take this one step further, the ease of management could be mitigated if VMware goes on to acquire a configuration management company it has [already invested in, namely Puppet](http://puppetlabs.com/blog/vmware-invests-30-million-in-puppet-labs).

Either way, if VMware acquires Cumulus or Big Switch, they would be getting some exceptional talent.  From a pricing perspective, it may be that Big Switch’s price tag would be lower than Cumulus’s, but hey, I’m just speculating. 

Feel free to comment below and let me know what you think.

Thanks,
Jason

Twitter: @jedelman8