---
layout: post
tags: devops, ansible, network automation
title: Getting Structured Data Back from CLI Devices
---

About 6 months ago, I wrote a post titled [Programmatic Access to CLI Devices with TextFSM](/home/programmatic-access-to-cli-devices-with-textfsm) that talked about what was possible with [TextFSM](http://code.google.com/p/textfsm/wiki/TextFSMHowto) and even some things that could be possible with Ansible.

The overall feedback about the Ansible module (aka *netget*) shown in that post was great --- it was ultimately taking input parameters such as the template file along with the CLI command to execute on the devices in order to return structured JSON data back using Ansible.  In other words, the module used Ansible and TextFSM to create a pseudo-API for accessing data in *traditional* (no API support) devices.

Since then, [we](http://networktocode.com) have been using TextFSM for a few customer projects and found out there was another capability within TextFSM to create what's called an "index" or a mapping between CLI commands, vendors, and TextFSM templates.  By integrating this functionality, instead of users needing to know what template to call, they can simply just send in the show command, while the index maps the command to the proper template!  Pretty sweet!

Needless to say, the Ansible module is officially [online](https://github.com/networktocode/ntc-ansible) and is now called *ntc_show_command*.

With the right amount of community support, it shouldn't be hard to build up the index to supports dozens of commands across different types of network platforms!  

Note: THIS IS NOT LIMITED TO NETWORK DEVICES, but it is using [netmiko](https://github.com/ktbyers/netmiko) (SSH) as it's underlying transport mechanism. 

Please check out the [GitHub repo](https://github.com/networktocode/ntc-ansible) - it should have what you need to get started.  The repo has an example of getting JSON data back for HP and Cisco switches using a single playbook and single task.

If there are any questions, please do not hesitate to reach out, and as always, feedback is always appreciated.

Thanks,
Jason

Twitter: @jedelman8

