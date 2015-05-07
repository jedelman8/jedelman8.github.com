---
layout: post
title: Nexus 7000 FAQ
tags: nexus, cisco
---

The Nexus 7000 is constantly evolving and there seems to be more and more design parameters that have to be taken into consideration when designing Data Center networks with these switches.  I’m not going to go into each of the different areas from a technical standpoint, but rather try and point out as many of those so called “gotchas” that need to be known upfront when purchasing, designing, and deploying Nexus 7000 series switches.


Before we get started, here is a quick summary of current hardware on the market for the Nexus 7000.

1. Supervisor 1
2. Fabric Modules (FAB1, FAB2)
3. M1 Linecards (48 Port 10/100/1000, 48 Port 1G SFP, 32 Port 10G, 8 port 10G)
4. F1 Linecards (32 Port 1G/10G, F2 linecards, 48 Port 1G/10G)
5. Fabric Extenders (2148, 2224, 2248, 2232)
6. Chassis (7009, 7010, 7018)

Instead of writing about all of these design considerations, I thought I’d break it down into a Q & A format, as that’s typically how I end up getting these questions anyway.  I’ve ran into all of these questions over the past few weeks (many more than once), so hopefully this will be a good starting point, for myself as I tend to forget, and many others out there, to check compatibility issues between the hardware, software, features, and licenses of the Nexus 7000.  The goal is to keep the answers short and to the point.

**Question:**
**What are the throughput capabilities and differences of the two fabric modules (FAB1 & FAB2)?**

**Answer:**
It is important to note each chassis supports up to five (5) fabric modules.  Each FAB1 has a maximum throughput of 46Gbps/slot meaning the total per slot bandwidth available when there are five (5) FAB1s in a single chassis would be 230Gbps.  Each FAB2 has a maximum throughput of 110Gbps/slot meaning the total per slot bandwidth available when there are five (5) FAB2s in a single chassis would be 550Gbps.  The next question goes into this a bit deeper and how the MAXIMUM theoretical per slot bandwidth comes down based on which particular linecards are being used.  In other words, the max bandwidth per slot is really dependent on the fabric connection of the linecard being used.


**Question:**
**What is the maximum bandwidth capacity for each linecard and does it change when using different Fabric Modules?**

**Answer:**

![Nexus Bandwidth Capacity](/img/7kbwcap.jpg)

*20 linerate ports through fabric

It is recommended to deploy at a minimum of N+1 redundancy with Fabric Modules.  For example, when deploying an M1 10G module that has a max bandwidth/slot of 80G, only 2 x FAB1s are required, but 3 will allow one to maintain the full 80G should there be a fabric module failure.  Nowadays with the FAB2s, Cisco is requiring a minimum of 3 either way, which is definitely good practice.

FAB1 & FAB2 modules are both forward and backward compatible.  For example, a FAB1 can be deployed with the F2-48port module, but would have a maximum throughput of 230G/slot versus the 480G/slot when using FAB2s.

**Question:**
**What is the port to port latency of the various 7K linecards?**

**Answer:**

![7K Latency](/img/7klatency.jpg)

N5K and N3K latency has also been included because many times if you need this info for financial applications or any other latency sensitive app, the comparison usually ends up expanding to include these platforms as well.

Note: from my research I’ve seen some conflicting information for latency for the M1 linecards.  I’ve seen general statements such as the M1 family has 9.5 µsec latency, whereas, I have seen one document that stated 18 µsec as well.  The one that had 18 was an older document, so I do expect some of the M1 numbers could be off.  If anyone has these, please feel free to share.

**Question:**
**Which Nexus 7000 linecards can connect to a Fabric Extender (FEX)?**

**Answer::**
Simply put, there are only two linecards that can connect to a FEX on a 7K.  They are the 32 port M1 and 48 port F2 linecards.  If you don’t have one of these, get one, or use a 5K :).

**Question:**
**Does the F1 linecard really not support Layer 3? How is that possible?**

**Answer:**
This is an interesting one, but the short Answer: is no, the F1 linecard does not natively support Layer 3.  Okay, so what does that mean? First, it is important to note the 7K architecture is different than that of other platforms such as the Catalyst 4500 or 6500.  These other platforms have a centralized (and distributed using DFCs on the 6500) forwarding architecture.  Remember the 6500 has a MSFC routing engine and PFC (where policies and FIB are located after they are built) located on the Supervisor and then pushed to the DFCs should they exist on the linecards.  The notion of a centralized PFC goes away with the Nexus 7000 and it is based off a purely distributed architecture – think the 6500 with DFCs without a centralized PFC. 

So to answer that question in the most direct way, and comparing it to what was just described, the F1 module does not have the capacity to locally store a distributed FIB table on the linecard.  The F1 was purposely built for advanced Layer 2 services including a technology called Fabric Path.

Now, with all that being said, it is still POSSIBLE to run layer 3 in a chassis that has F1 alongside M1 linecards.  There is a notion of proxy layer 3 forwarding that exists.  This allows SVIs to be created on the system (technically, that will exist on the M1s), assigned an IP address, and then ports on the F1 to be assigned to that VLAN in order get “proxy L3 forwarding” for hosts that directly connect to the F1.  It is NOT possible to configure a routed port on any F1 port.  If you’re curious on how this happening, the F1 linecard is building a Port-Channel within the “backplane” that will connect to up to sixteen (16) forwarding engines on M1 cards.  This means the maximum capacity for proxy layer 3 forwarding is 160G because each forwarding engine maps back to a forwarding/port ASIC on the M1 linecards.  Sometimes, when I describe this, I'll also use the analogy of thinking about the F1 as an IDF switch and M1s as a Core switch, so even though your IDF switch is L2 only, you can still route by getting switched back to the Core.  

Note: it is possible to explicitly configure which forwarding engine’s should be used for F1 linecards.

**Question:**
**What are the design options available when connecting a Nexus 2000 to Nexus 7000s?**

**Answer:**

I’m going to keep this short on the preferred method as it is now supported as of NX-OS 5.2.  If you have 2 x 7Ks, 2 x 2Ks, and a server, the recommended design would be connect your server to both 2Ks.  Each 2K would connect to ONE 7K.  That is very important; you CANNOT dual-home a 2K and connect it to 2 separate 7Ks.  I’ve heard this may NEVER be supported.  It doesn’t seem logical, but you don’t lose anything single-homing the 2K to 7K.  Should any link or device go down, half of the bandwidth still remains up.  Prior to 5.2, LACP was not supported between the 2Ks and server and basic active/standby NIC teaming was required.

**Question:**
**Can different linecard types (M1, F1, F2) exist in the same chassis?  If so, what functionality is gained or lost?**

**Answer:**

Major design caveat:  F2 linecards require a dedicated VDC if they are in the same chassis as ANY other M1 or F1 linecard.   This one isn’t pretty, but it is what it is for now.  This means you'll need the Advanced Services License to enable the VDC functionality as well.

For other critical design caveats, please take note of the following:  Mixing and matching F1 and M1 in the same chassis works fine.  The biggest caveat is the L2/L3 support issue as described above.  Remember, F1 cards do NOT support L3 routed ports and only support L3 via proxy routing through M1 linecards in the chassis.  In large data centers, other details need to be examined such as the MAC table size.  The supported MAC table sizes are different on each card ranging from 16k to 256k MACs, so should the data center be increasing in size by means of virtualization adding 100s to 1000s of VMs, this should be examined a bit further.

**Question:**
**Does the Nexus 7000 support MPLS?  If so, are there any restrictions on software and hardware?**

**Answer:**
Yes.  The Nexus 7000 supports MPLS as of NX-OS 5.2(1) with an M1 linecard.  Note:  the MPLS license is also required.  F1/F2 modules DO NOT support MPLS.

**Question:**
**What software, hardware, and licenses are required in a Nexus 7000 OTV deployment?**

**Answer:**
OTV was introduced in 5.0(3).  Any M1 linecard and the Transport Services Package license is required. F1/F2 modules DO NOT support OTV. 

Note:  based on the low level design of OTV, the Advanced Services License may be required to enable VDCs to support the OTV deployment. 

**Question:**
**What software, hardware, and licenses are required in a Nexus 7000 LISP deployment?**

**Answer:**
LISP was introduced in 5.2(1).  It requires the 32 Port M1 linecard(s) and the Transport Services Package license. Other M1 modules and F1/F2 modules DO NOT support LISP. 

**Question:**
**What software, hardware, and licenses are required in a Nexus 7000 FCOE deployment?**

**Answer:**
FCOE for the 7K was introduced in 5.2(1).  32 Port F1 linecard(s) are required;  48 port F2 will support FCOE sometime in 2012. 

Note:  One or more licenses are required for FCOE on the 7K.  The FCOE license is required per linecard which inherently offers the ability to create the storage VDC (that is required), while the SAN License is required for the system Inter-VSAN routing and fabric binding.  The Advanced Services License that normally enables VDCs is not required.

**Question:**
**What software, hardware, and licenses are required in a Nexus 7000 FabricPath deployment?**

**Answer:**
FabricPath was introduced in 5.1(1).  It requires the use of either the F1 or F2 module along with the Enhanced Layer 2 Package License.

Note:  While both F1 and F2 modules can run Fabric Path, they cannot be in the same VDC, so one would probably choose one or the other for a FP deployment.

**Question:**
**Are special racks needed for the Nexus 7000 switches?**

**Answer:**
Four (4) post racks are required for the 7010 chassis and 7018 chassis. There are several racks that were purpose built for the 7K, but are not “required.”  These are documented further in the hardware installation guide for the 7K.  Note:  if not using the purpose built racks, be sure to measure the depth required for the N7K.  I have ran into situations where the depth was too short and the rack needed to be extended out further delaying the deployment process.  Not fun!

The 7009 was built to ease migrations from 6509s, so a 2-post rack works quite well for these, as it is the same exact form factor as the 6509/E.
