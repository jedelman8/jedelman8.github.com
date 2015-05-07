---
layout: post
title: Giving a Monkey a Loaded Gun
tags: automation
---

Automating the configuration, provisioning, and management of particular workflows for cloud gets a lot of attention these days.  While automation makes perfect sense for deploying workloads faster, there are also other areas where automation can be leveraged to improve the overall operational efficiency of the IT Ops team. 

One of these areas is automating the validation of configuration changes.   This could mean validating changes deployed via the CLI for existing networks or validating changes made by SDN controllers for those new shiny physical AND virtual networks.  It doesn’t matter.  [Connectivity tests can also be automated](/home/network-test-automation-with-ansible).  Much more can be automated than configuration and policy for data centers.

I remember doing annual power shutdowns of the data center and IDFs where I worked years ago.  I remember doing OS upgrades on critical network devices.  I also remember the chaos and the amount of people that needed to be on a bridge validating “everything looked okay” when the devices came back online.  Was everything always okay?  Hardly, but it wasn’t until business started on the following Monday, those one off issues were uncovered and fixed.  If there were tools verifying routing tables, interfaces, and general state of all IT systems (pre and post upgrade/reboot), it would have made everyone a little saner.   

The interesting thing about automation --- it is in fact risky automating configuration changes.  Some may call it scary and some call it giving a monkey a loaded gun.  Do you REALLY want to do that?  Is there a safety switch included?

**Note:**  please always make sure you test and QA any new tool/product.  Just figured I’d state the obvious here.

![Monkey](/img/monkey-gun.jpg)

Many thanks to Ryan Hughes (@angryjesters)  who educated me on the monkey with a loaded gun analogy and how it relates to automation ;).  We gave an internal presentation together last year and this was one of my favorite parts of the presentation.

Rather than just writing about all of this, I felt it was best to show an example of what I’ve been describing here (see demo/video below).  With more demos out there, the  hope is there will be more “ah ha” moments to see the benefits that various forms of automation can offer organizations regardless of vertical, size, and scale.

The video below is a demo of some new custom Ansible modules I created that automate the validation of OSPF adjacencies.  Data is automatically extracted from each router and then compared to determine if the routers are configured properly for OSPF.  You’ll see towards the end of the video I modify one of the interface’s MTU and re-run the Ansible playbook and the playbook automatically re-configures the MTU to what it should be.  So, this playbook actually auto-validates, but also auto-configures.  The re-configuration part is purely optional, but thought it would be good to show what can be achieved with some of the DevOps tools out there such as Ansible.

[YouTube Video](https://www.youtube.com/watch?v=RYFcpuCYBKk)

While usually overlooked, validating and learning are SIGNIFICANT pieces to the puzzle to create the ultimate feedback loop in terms of IT optimization.  As always, would love to hear feedback and general commentary in the comments section below.

Thanks,
Jason

Twitter: @jedelman8

**P.S. Please ensure you don’t have any monkey’s operating your company’s automation platforms or IT systems.**





