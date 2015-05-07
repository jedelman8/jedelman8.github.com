---
layout: post
title: Internal Networking for the new UCS Modular Chassis 
tags: cisco, ucs
---

At the end of the day, any announcement that is NON networking focused NEEDS a networking focus.  I just attended the UCS Grand Slam launch where there were a few announcements for UCS including the new UCS mini, capacity optimized UCS, and the UCS modular chassis (M series).  The latter was of the most interest to me.
The new M series de-couples CPU and memory from other peripherals on board including networking and storage.  CPU and memory exist on a replaceable / upgradeable cartridge and can be upgraded without touching the network or SSDs on board.  This smells and feels like the disaggregation happening in the Open Compute Project (OCP), but of course, the Cisco solution can be managed with all existing and new UCS systems via UCS Manager.  This could definitely be the start of a new computing paradigm for the Enterprise. 

### Platform Overview

Each UCS M series chassis has 8 cartridges in a 2 RU form factor.  Each cartridge has two (2) quad core CPUs and 64GB memory, thus each server has a single quad core CPU and 32GB memory.  There are also 4 SSDs and dual 1400W power supplies per modular chassis.

What I found interesting is the networking aspects of what’s going on here with this platform.

### Network Summary

* Each server connects via two (2) PCI-E lanes into a central ASIC called System Link Technology.  The lanes are bonded at the hardware layer so a flow greater than a single lane is possible.
* Each PCI-E LANE is [985MB/s](http://en.wikipedia.org/wiki/PCI_Express#Lane) (v3.0), so doing some complex math, this means the connectivity per server in aggregate is either 15.76 Gbps.  
* This System Link ASIC is the integration point from the CPU/memory to the other peripherals on board as well as the outside world (storage controller also connects to this ASIC)
* Two 40G QSFP interfaces also connect to the same System Link ASIC.  These 40G interfaces are used to connect northbound to UCS Fabric Interconnects.  The UCS FI’s are a requirement.
* This does in fact mean the System Link ASIC can do Ethernet and PCI-E.  Interesting as we see new and emerging networking technologies take off.

Update 9/5/204: Cisco verified it is PCIe 3.0 being used and James Harr kept me honest in that the PCIe spec is in bytes, not bits!  Updates were made.

If you would like to add any more detail to this, please feel free to comment below.

Thanks,
Jason

Twitter: @jedelman8