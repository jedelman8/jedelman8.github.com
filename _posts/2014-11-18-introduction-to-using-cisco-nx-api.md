---
layout: post
title: Introduction to Using Cisco NX-API
tags: cisco, nexus, nxapi
---

I've posted a few times in the past about Cisco's NX-API and realized I hadn't provided any guidance on how to get started using the API itself.  In this post, I share two videos that are meant to serve as a quick start to those who don't have a development background and are looking to test NX-API.

The first video looks at the NX-API sandbox and how you map the data represented in the sandbox back into objects that you can use while working in Python.
The second video shows where to get the modules that I use in the first video, namely xmltodict and device.py.

Note: the device module that I use is primarily used with XML data being returned from the device.  The easiest thing for those who want to test is to follow the steps outlined in the videos although there are mechanisms to switch to JSON.  This device module does not support json-rpc (as that is still fairly new in NX-API).

And, don't forget, you'll need to connect to your Nexus 3K/9K via the management interface to work with NX-API.


### Videos

#### [Introduction Part 1](https://www.youtube.com/watch?v=zYQjTE9KJzA)

#### [Introduction Part 2](https://www.youtube.com/watch?v=ppd0VbI7xxQ)


Other NX-API links to check out:

[Cisco Nexus 9000 NX-API][1]
[Cisco NX-API 1.0 Update][2]
[Prescriptive Topology Manager (PTM) support with NX-API on the Nexus 9000?][3]
[Leveraging Cisco NX-API with Ansible to Make Your Life Easier][4]
[Automated Network Diagrams with Schprokits & AutoNetkit][5]

[1]: http://keepingitclassless.net/2014/02/cisco-aci-nexus-9000-nxapi/
[2]: http://keepingitclassless.net/2014/10/cisco-nxapi-10-update/
[3]: http://www.jedelman.com/home/prescriptive-topology-manager-ptm-support-with-nx-api-on-the-nexus-9000
[4]: http://www.jedelman.com/home/leveraging-cisco-nx-api-with-ansible-to-make-your-life-easier
[5]: http://www.jedelman.com/home/automated-network-diagrams-with-schprokits-autonetkit


If you have any questions or would like to see other short videos, feel free to let me know.

Thanks,
Jason

Twitter: @jedelman8

