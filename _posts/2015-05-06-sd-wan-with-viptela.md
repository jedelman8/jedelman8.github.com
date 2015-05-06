---
layout: post
title: SD-WAN with Viptela
tags: sdn, viptela
---
Software Defined Wide Area Networking (SD-WAN) is bubbling up to be one of the prime use cases of SDN.  The vendors that fall into the SD-WAN bucket often include Glue Networks, Nuage, Viptela, CloudGenix, VeloCloud, etc.  As you dive into each of the solutions from the vendors, you may realize that some are truly unique technologically and some may just be offering a better way to manage existing wide area networking equipment (which is still a huge value add).

In this post, I’m going to give some background on what is driving me to deploy an SD-WAN solution.  Follow up posts will cover the deployment a bit more technically.


### Requirements

Since I now have equipment in a colo, moved into a new office, and of course, have the home office, I figured it may be a good idea to look at some of these SD-WAN technologies.  In reality, my requirements have a mobile 4th site too that will be used when doing consulting and training onsite at customers to give dynamic site to site access just back to the colo.

To be perfectly honest, I didn’t have strict requirements – they are probably equivalent to those of a small IT organization looking to deploy and support a few branch sites.

Few of the requirements:

- Simple to configure and setup
- Simple to manage and operate
- Must secure all traffic in transit as these tunnels will be over the public Internet
- Have the ability for a “travel” device that is plug-n-play and can automatically join the VPN
- APIs – while this may not be a requirement of a typical small organization, it was for my company as we want to programmatically make changes going forward
- Have the ability to run VRF equivalent functionality over the site to site tunnels – as the colo is built-out, we’re going to have different security domains and want to ensure traffic is easily isolated
- Limited amount of hardware on site – ideally just the routers / security appliances, so it should offer a cloud managed portal

### Going with Viptela

I had sat in Viptela presentations a few times in the past and their solution seemed intriguing.  One of things they are delivering on is building a scalable control plane for very large scale VPN deployments.  As it turns out, many of their initial customers had significant challenges scaling into the 1000s of sites using traditional site to site VPN technologies.  That’s actually one of the major problems Viptela fixes with their own control plane, but even while there is some secret sauce with how they perform tunnel setup, it really didn’t play a factor for me as I won’t have scaling issues :).

Just due to the simplicity of the setup, it was enough to choose Viptela for me, so I cannot give an adequate comparison of all of the other SD-WAN vendors from a usability perspective.  So, it is now definite that I’ll be deploying their SD-WAN solution in the very near term.  

Over the next few weeks, I’ll try and post a few more articles with my progress on getting the Viptela devices turned up and into production.  

If you have any questions or comments, feel free to write in below!

Thanks,
Jason
@jedelman8

