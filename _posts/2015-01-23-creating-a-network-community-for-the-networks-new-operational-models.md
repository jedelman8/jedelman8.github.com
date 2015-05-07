---
layout: post
title: Creating a Network Community for the Network's New Operational Models 
tags: community, sdn, devops
---

The way in which networks are configured, deployed, and managed is changing.  The network industry is in a shift from managing devices box by box via the CLI to having more centralized ways to manage and deploy devices.  While the CLI isn’t going away anytime soon, we can look at the two operational models that are gaining traction within the network community.

### SDN Controllers

SDN controllers do two major things that increase operational efficiencies.  They offer a central point of management and visibility for the network team, but also offer a single point of integration for 3rd party systems – these systems could be anything from cloud management platform, monitoring or automation systems, to native business applications.  Note: even when there are controllers being used by a human, there is risk.  There is the risk of clicking the wrong button, forgetting the order of operations of which buttons needs to be clicked, etc.  This doesn’t go away.  Change control windows still have risk.

### DevOps for Networking

We’ve also seen an increased amount of focus on the intersection of DevOps and networking. I’m really referring to DevOps automation tools and the network.  For example, Puppet announced $40M in funding in June of 2014 and they announced they wanted to take what they’ve been doing in the compute world to storage and networking.   

There is also a significant focus on leveraging Ansible to manage network devices.  I’ve written about Ansible a ton and the real value here compared to other tools is due to Ansible’s simplistic playbook language (YAML) and more importantly for network engineers, Ansible DOES NOT require a software agent on the network device.  This is native out of the box.  It has a plug-in architecture that can work with any API.

From a personal perspective, I believe 100% having the network integrated to DevOps tooling is required.  While SDN and DevOps for Networking are often seen as two different operational models, they do not have to be.  This is because if you are deploying a controller based solution, having a way to confidently deploy the same configuration and policy on a single controller or a group of controllers (prod/DR, Site A/Site B) is still needed.  The ability to extract data to feed into other systems is still needed.  DevOps tooling can drive SDN solutions through the REST APIs exposed from controllers.

Additionally, DevOps tooling streamlines the way configurations are built and modeled.  Often times, these tools are using some type of templating system.  Such systems are Mako, Jinja, and ERB.  Once configurations are broken down and properly templated, it becomes trivial (at least easier) to migrate between vendors, review configurations between team members, not to mention the inherent benefit of predictability – no CLI commands and no clicking around directly on a network device.  And most important, one can start to focus on the configuration, policy, and desired state without being bogged down by syntax or CLI commands.

# Community

Saving the best for last, we have community.  Ansible and Puppet are examples of two great open source projects led by two growing companies.  I’ve had the pleasure of working with the Ansible community via google groups and their IRC chat, and they are always super responsive.  The only thing that would make a community more awesome is if there was a vendor independent community focused on not just DevOps automation, but also networking.

It is for this reason, I’d like to launch a community for  network engineers by network engineers who are interested in programming, DevOps, network APIs, REST APIs on controllers, general automation, and continuous integration.  Calling it a community may be a bit aggressive at this point, but in reality, just using a Google Group would be a great start.  It should be an email alias that any person can use to share information, share links, ask questions around emerging APIs like Cisco onePK, Arista eAPI, Cisco  NX-API, Palo Alto FWs REST APIs, or even the APIs on Cisco’s APIC or VMware’s NSX platform.  It could be about how to use Ansible for templating.  It doesn’t matter what the product or tool is – the goal is to bring together a group of people who are doing – people who are learning together and automating together.  I can’t promise anything will become of it, but it sure as heck can’t hurt trying!

If you are interested, please sign up for the Google Group – the group name is called networktocode (“network.toCode”).  You can click [here to get going](https://groups.google.com/forum/?hl=en#!forum/networktocode)!

![Taking Out the CLI](/img/takingoutthecli.jpg)

Thanks…and here is to the future of networking!

-Jason

Twitter: @jedelman8