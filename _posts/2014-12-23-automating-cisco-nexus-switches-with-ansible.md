---
layout: post
title: Automating Cisco Nexus Switches with Ansible
tags: cisco, nexus, ansible, nxapi
---

In previous posts, I’ve written about using Ansible for network automation.  Few of them can be found [here][1], [here][2], [here][3], and [here][4].  In one of the [posts][5], I had a video that was automating Cisco routers with Ansible, and was using onePK as the API to communicate to the device.  In this post, I’ll be focusing on automating Nexus switches – this means each of the Ansible modules will be using NX-API to communicate with the device.  This also eliminates the need for the users of these modules to know Python as they’ll be using the Ansible platform for their specific automation needs.
While the demo below is for configuration automation and shows what can be done in just a few seconds, it needs to be understood that automation is much more than pushing configurations.  I hope to show some of this first hand by doing more interesting things as it pertains to data gathering, verification, troubleshooting, that do increase speed and agility, but also [predictability][6].

[1]: /home/ansible-for-networking
[2]: /home/demo-using-ansible-for-network-automation
[3]: /home/giving-a-monkey-a-loaded-gun
[4]: /home/leveraging-cisco-nx-api-with-ansible-to-make-your-life-easier
[5]: /home/demo-using-ansible-for-network-automation
[6]: http://keepingitclassless.net/2014/12/automation-isnt-just-speed/

The following video shows how Ansible can be used to automate interfaces and VLANs on Nexus switches.   This will be the first in a series of videos that will show how Ansible can be used to stand up (config, deploy) a complete leaf/spine topology using Nexus switches.

## Video
[YouTube Video](https://www.youtube.com/watch?v=CvrcLO8y07o)

For those who do not watch the video, here are a few screen shots that show the Ansible hosts file and playbook being used in the demo along with a few slides that describe/show the parameters that can be used for each module.

#### hosts file

![nx1](/img/nx1.png)

#### playbook

![nx2](/img/nx2.png)

#### nxapi_interface module overview

![nx3](/img/nx3.png)

#### nxapi_vlan module overview

![nx4](/img/nx4.png)

#### Code Access / Roadmap

![nx5](/img/nx5.png)

These modules are posted on GitHub and can be found [here](https://github.com/jedelman8/nexus_ansible).  If anyone wants to chat or dive into Ansible, these specific modules, or NX-API in a bit more detail, feel free to reach out.  I would love to catch up.

Thanks,
Jason

Twitter: @jedelman8
