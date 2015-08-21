---
layout: post
tags: big switch, nfd
title: Big Switch Improves Day to Day Network Operations
---

Big Switch recently launched major updates to their products Big Cloud Fabric (BCF) and Big Monitoring Fabric (BMF), formerly Big Tap.  This post isn't going to cover the updates or the products from an architectural standpoint, but rather two specific features that are meant to help general day to day network operations.

## Command & API History

The first feature is simple – it shows command history, but also API history across the entire Big Cloud Fabric (BCF).  The feature is accessed through the central UI of the BCF controller and you can simply look at the last N commands or APIs that were executed on the system.  The great thing is that you don’t need a separate AAA system to capture the commands being made and should you want to see the API calls being generated from the CLI commands (because remember the CLI is just an API client), you can also view them.  If the CLI isn't being used, you can also still see each API call that has been recently made on the fabric.  It’s my understanding that there is a certain amount of storage dedicated to this function so when the space does fill up, the history will start to roll over.  Another nice fact is that the CLI and stats storage is using [Elastic Search](https://www.elastic.co/products/elasticsearch).

## The Origination Flag

The second feature is something that I haven’t heard before from any vendor.  We all know that modern platforms have APIs, but also usually maintain a CLI for those engineers that will still perform manual, or one-off, changes.  When a device (or controller) has an API, there are better odds that its functionality will be automated.  This is the case with Big Switch’s Big Cloud Fabric as it’s often used as a network fabric for OpenStack and VMware workloads meaning the BCF is programmatically configured by VMware’s vCenter or an OpenStack cloud controller (neutron-server).  But what if in the same BCF system, you need to make a one-off change that either cannot be done via the cloud management platform or just really prefer to do it via the CLI.  How do you know or keep track which changes were done via vCenter, OpenStack, or manually via the CLI? The great thing is Big Switch supports having an *origination* flag so you can keep track of how the configuration was applied to the fabric.  There is also a config synch that the BCF maintains such that if there is a conflict, the cloud platform config will override a manual change!  Very nice.

In the past, I’ve heard vendors say, you can use the same product for traditional and automated workflows, but have not seen any way to distinguish between the instantiated policies.  The safer approach is always to automate and not make manual changes on a device that is being managed programmatically, but if budget doesn’t allow for separate instances, etc., the approach to deploy origin tags based on how a configuration is applied is pretty slick, IMHO.

Big Switch recently presented at Network Field 10 – if you want to hear about Big Cloud Fabric and Big Monitoring Fabric in greater detail, check out the [videos](http://techfieldday.com/appearance/big-switch-networks-presents-at-networking-field-day-10/).


Thanks,
Jason

Twitter: @jedelman8

