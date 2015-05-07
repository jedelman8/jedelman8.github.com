---
layout: post
title: Integrating Spirent into an Automated Workflow Test Methodology
tags: spirent, automation, tdd, ci
---

I’ve spent the last few days getting briefed by several vendors in Silicon Valley.  They include A10, Big Switch, Brocade, Cisco, Gigamon, Nuage, Pluribus, Spirent, and Thousand Eyes.  Over the next few weeks, I’ll try and get a few posts out about the briefings, but for the first one I wanted to focus on Spirent.  Many are probably aware that Spirent provides packet generators and while that’s what they sell and are really good at, it’s the strategy, vision, and software integration with their products that was extremely intriguing.  

I’ve engaged with many customers over the past 10 years and the majority have never felt a real need to test performance.  It was and is usually very easy to over provision hardware when it comes to Layer 2 & 3 switching.  This is still the case for the most part too – there are 1 RU and 2 RU switches that can forward traffic faster than those big monster boxes of just a few years ago.

### Why Test Now?

There are network functions being virtualized from almost every vendor out there --- this usually falls under the Network Functions Virtualization (NFV) trend.  It’s common to hear about virtual routers, load balancers, firewalls, and other services appliances, but it is still uncommon to see them in deployments.  Why?  Fear, uncertainty, and doubt (FUD) and vendor limited performance/features have a lot to do with it.  How will the virtual firewall perform?  How well does it match the data sheet numbers?  Can a virtual load balancer handle the SSL offload required for a particular environment?  What happens if more memory or CPU is added to the virtual appliance – does it help or hurt?

That all said, software appliances as well as hardware appliances aren’t going anywhere.  They will each have their place in the network, but for the 1000s of small to mid-size environments out there, there is no reason that virtual appliances couldn’t be used.  And for physical appliances, do you really want to over-provision?  As bandwidth speeds are hitting high density 10G, 40G, and 100G on switching platforms, this is NOT the case on L4-7 appliances like FW & LB.  If you try and match those speeds on L4-7 devices, you’ll need to spend millions of dollars.  This is another reason testing totally makes sense nowadays.

### How do you validate the L4-7 appliances?

Clearly, Spirent can be used to validate the performance of the appliances.  If you want to deploy a virtual FW/LB, you can probably even use the 30 or 60 day trial version of that product to test with.  It is worth noting Spirent has virtual appliances that can be used to test traffic rates up to 5Gps.  Last time I checked, 5Gbps is still greater than most Internet circuits, thus these appliances can be used to potentially test load balancers and firewalls that will reside on the perimeter or within the data center of most Enterprises.

It’s the process, time, and effort that usually slows things down though.  To do this (before the virtual Spirent appliances came out) in the past, you would have to rent/lease/buy physical appliances, get them racked and cabled, learn all about Spirent, and hope you are performing the right test.  Let’s say all that happens to go smooth and shortly after, you make a change to your firewall and you no longer have the Spirent appliances.  Do you re-validate the performance after the change (and assume the change won't affect performance) or was the initial perf test just about seeing what the max throughput rates were on the appliance?  While a one-time test is good, what could make it even better?

In reality, the best case scenario is an integrated workflow that includes an engineer pre-building config changes, having the changes be reviewed by others on the team and other cross-functional teams as needed, have the changes deployed in a pre-prod (or prod) environment, and following the changes, have unit tests performed that include both functional and performance tests.  It’s this type of vision that Spirent shares, which is pure awesomeness.  It’s rare to see any networking company advocate for or have a Continuous Integration (CI) strategy integrating with tools like GitHub, Gerrit, or Jenkins.

Spirent is not the norm here and are definitely showing thought leadership.  Spirent has developed a Jenkins plug-in that will help to build the integration tests that need to happen after a given change, or code drop, to a git repository like GitHub.  You don’t need Git integration to use what Spirent has developed, but it helps to give a good example for a complete workflow.

*Note: I actually planned to do a short Continuous Integration (CI) post before this one, but that didn’t happen.  Expect that to come soon and that may give more clarity on what’s possible with GitHub + Jenkins integration.*

### What does this mean?

You would build out the workflow and not have to spend countless hours performing the tests! Jenkins would kick off all required tests.  Tests could include anything you’d like to verify/validate configs and features, etc., but using the Spirent Jenkins plug-in, it would communicate to the Spirent device(s) to perform the required performance tests.  Again, this eliminates the need for a person to be running 10s and 100s of manual tests, which is just huge.  As CI methodologies become more popular in the network space, we’ll be able to know exactly what to expect in terms of features and performance after each and every change, while ensuring the changes are successful and don’t break anything.  Ideally, this gets done on the prod gear during the change window, but to get things started, there is no reason this can’t be done on lab kit especially if you’re testing virtual appliances!

It’s also worth noting Spirent has a virtual test center that customers can use as well to test particular configurations.  For example, you can test Open vSwitch, VMware VSS/DVS, or whatever you’d like. If it’s software like a FW/LB, just upload and kickoff an automated test on their hardware.  If you want to test on YOUR particular hardware, which is more in line with what I’d recommend, you can still use their Jenkins plugin to assist in building out a proper test methodology.

All of the sessions were recorded this week - feel free to check out the [Spirent videos](http://techfieldday.com/appearance/spirent-presents-at-networking-field-day-8/).

If you are interested in testing out virtual appliances, let me know.  This is an area I may be launching a service offering around in the near future based on customer feedback.

Thanks,
Jason

Twitter: @jedelman8

Disclaimer: Spirent was a presenter at [Networking Field Day 8](http://techfieldday.com/event/nfd8/).  They did not ask for any consideration in the writing of this review nor were they promised any review.  The conclusions and analysis contained in this post are mine and mine alone.


