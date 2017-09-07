---
layout: post
tags: automation, ansible, intent
title: Intent-Based Network Automation with Ansible
---

The latest in all the networking buzz these days is Intent-Based Networking (IBN).  There are varying definitions of what IBN is and is not.  Does IBN mean you need to deploy networking solely from business policy, does IBN mean you must be streaming telemetry from every network device in real-time, is it a combination of both?  Is it automation?

This article isn't meant to define IBN, rather, it's meant to provide a broader, yet more practical perspective on automation and intent.

# Intent isn't New

One could argue that intent-based systems have been around for years, especially when managing servers.  Why not look at _DevOps tools_ like CFEngine, Chef, and Puppet (being three of the first)?  They focused on desired state--their goal was to get managed systems into a technical desired state.

**If something is in its desired state, doesn't that mean it's in its intended state?**

These tools did this eliminating the need to know the specific Linux server commands to configure the device--you simply defined your desired state with a declarative approach to systems management, e.g. ensure Bob is configured on the system without worrying about the command to add Bob.  One major difference was those tools used the term "declarative" and not "intent-based" when it comes to industry terms and hype.  That said, it's actually quite a solid approach to systems management.

# Using DevOps Tools for Networking

Using a declarative approach for systems management was solid enough that eventually these tools were extended to manage network devices.  The only caveat was the companies that developed these tools never employed anyone focused on network integrations.  Then along came Ansible--they were the same in that they employed no one that developed network integrations (circa 2014-15).  There was a major difference between Puppet/Chef and Ansible though.  It was that Ansible was dead simple and the network community drove the initial Ansible <--> network device integrations.  Even vendors like Juniper and Arista wrote their own modules.  Finally, Ansible made investments and continues to do so, and now has a full team dedicated to networking.

> Since I mentioned all other popular "configuration management" tools, there is even the newest of these tools: Salt by SaltStack, that has a growing interest in the community to be used for network automation.

# Intent-Based Network Automation with Ansible

Let's get back to Ansible.  Ansible is interesting because it's often debated if it's _imperative_ or _declarative_.  This partially goes back to defining something as intent-driven or not.  More on this later though.

Major point: **certain _tools_ are not tools--they are mere platforms that you can build on**.  This is how I see Ansible.  If you look at a single task (in Ansible terminology) today, you may in theory be using CLI commands or making specific API calls.  What's great about this is that you know 100% what is being sent to the device. What's not so great about this?  It may not be considered intent-based by industry pundits.  However, platforms like Ansible have building blocks.  

Within Ansible, there are modules, tasks, plays, and roles that allow you to build your own abstractions that perform how _you_ need them to fit your environment.  This means you can write an Ansible playbook to automate your network that enforces your precise intent.  Of course, if you look inside the playbook, you may see _some_ imperative tasks, but who cares?  This is why there are platform architects (who build robust playbooks) and users of the system (who execute the playbooks).

**You should be able to define your desired state according to your Enterprise standards.**

# Using CLI Commands to Drive Intent

All we do at [Network to Code](networktocode.com) is network automation.  If all we had was something that could be used for greenfield deployments or that only worked in the WAN, Campus, **or** DC, we wouldn't exist as a services company.  Our major selling point is that we make network automation consumable and something that can be adopted and deployed in a gradual, yet effective, fashion across a wide variety of network types.  This means using CLI commands to even drive intent--let's face it, it's the world we live in. 

If there is any solution categorized as Intent-Based Networking (open source of commercial), I guarantee you it's using CLI commands if it's managing devices such as Cisco Nexus, IOS, or Arista EOS.  All this means is that the solution built more logic on top to enforce intent, perform real-time checks, and allows you to see and mange this with a slick UI.

# Network Intent with NAPALM

This post wouldn't be complete without mention of NAPALM.  NAPALM at its core is a Python library that was built to offer a uniform way (in Python) to manage network device configurations.  While this isn't meant to be a primer on NAPALM, there is a way to declaratively manage full device configurations with NAPALM.  This means if your desire is to have a particular running configuration on the device, you can use NAPALM to do so.  The subtle, but major difference here to traditional automation, is you do not have to issue any "no" or "delete" commands to do this.  

You literally focus on what your intent is with CLI commands, which in my opinion isn't a bad thing because CLI devices is what's mostly deployed today in production environments (forget about what transport is used - CLI over API or CLI over SSH...it's still being used).  Looking back, NAPALM was arguably the first networking library that offered intent (but uses declarative in their terminology just as the initial DevOps tools did).  Again, the real value here is that NAPALM is a collection of device drivers, so why not build atop NAPALM.  In fact, any commercial tool is using something equivalent like NAPALM to manage device configurations.

## Using Ansible and NAPALM

Because Ansible was extensible, there were NAPALM modules written pretty early on, pre-dating any network module in core today.  We could go onto say that Ansible has been doing IBN for over several years now!  In either case, systems have building blocks.  These days you can use native modules in Ansible core (much more of this coming with Ansible 2.4), or NAPALM modules with Ansible to write pretty robust playbooks--each playbook could do something like manage a configuration of device with intent in mind or something that makes adoption easier, managing each feature with intent in mind!  

**You shouldn't have to change your Enterprise standards to adopt automation.**

# From Automating Configurations to Automating Intent

This brings me to another point and question not often talked about when it comes to automation, configuration drift, intent, and declarative configuration management. 

If you're managing network devices, what is the source of truth (SOT)?  Where are the desired (or intended, if we want to use current terminology) configuration inputs held?  Is it in a custom database? IPAM solution? YAML files?  Any answer is better than, "_it's the network device and that's the only thing we trust!_"  However, after you consider where the SOT is and where you want it to be, you have to ask yourself, what about "additional configurations" that are on the device after you've automated your desired configuration?

* What if you desire 5 BGP neighbors and those 5 exist, but there are also 2 others?  
* What if you need 10 VLANs on each switch, but there is also an additional 2-3 on each switch?
* What about SNMP community strings?  Do all of your SNMP tools work, but you still see the aging comm strings for the past 20 years that you're afraid to delete?

We can ask these types of questions for every feature on a network device.  This is why there is actually a major difference with what we refer to as basic network automation (ensure your desired configuration exists) and fully declarative intent-based management per feature or per device (ensure your intended and desired configuration exists and remove all other configurations not in your SOT).

# Using Ansible as a Platform

If you treat Ansible as a platform, you get all of this today.  It's up to you to choose what level of abstraction is right for your organization and the users of the platform.  For example, we at [Network to Code](networktocode.com) have been hard at work deploying solutions like this leveraging Ansible as a platform.  We've deployed Ansible Tower (self-service via Tower Surveys) on top of Ansible if that meets customer requirements, and we've developed custom integrations (including front-ends) that are more network centric that sit above Ansible (or Tower).

> Note: the alternative is to use Ansible as a power tool and not as a platform, in which there is still great value, but it's then just automating CLI commands faster than you. 

**Realize there is a difference using something as a platform or a tool.**

### Tabling Operational State

This post didn't cover operational state as that's a bit more complex _to enforce_ and then you get into event-driven and auto-remediation systems, but in all of these tools and platforms, you have the ability to collect any information from the device (config or operational state), and then based on that data, work through your logic and intent-based deployment.

# Let's Meet at AnsibleFest SF 2017

A few of us from the NTC team are at AnsibleFest in SF tomorrow, September 7.  If you're interested in hearing more about our model-driven intent-based network automation services and solutions with Ansible, feel free to stop by our booth!  

# Network Automation with Python & Ansible Training 

In the spirit of AnsibleFest, NTC is offering 50% off any of our [public training courses](http://networktocode.com/products/training/) until 9/15.  Just email **info@networktocode** asking for your promo code. 

Happy Automating!

-Jason (@jedelman8)




