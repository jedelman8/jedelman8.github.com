---
layout: post
tags: automation, ansible, cisco
title: Using the Ansible ios_config Module
---

I get asked often on how to perform specific network automation tasks with Ansible.  There were a few questions recently pertaining to the `ios_config` module within Ansible core, so I decided to record a video to show different options you have when using it to deploy global configuration commands on IOS devices.

Here is a summary of the four (4) options covered:

1. Embed commands in your playbook and reference them using the `commands` (or lines) parameter.
2. Use the `src` parameter and reference a configuration file with one or more commands in it.
3. Use the `src` parameter and reference a Jinja2 template such that it inserts variables into the template, creating a list of commands, and  deploys them to a device.
4. Use two tasks.  In Task 1, use the `template` module and reference a Jinja2 template to auto-generate a configuration file.  In Task 2, use the `ios_config` module and reference the config file built in Task 1 to deploy the commands from the file.  This is often used instead of option #3 since it allows you to store/view the config file before deploying fully de-coupling the build and deploy processes.


[![Using the ios_config module](/img/ios_config.png)](https://youtu.be/WXLUgDmvHDI "Using the ios_config module")


The video does assume some existing knowledge on using Ansible.  The goal was to highlight different options available to you when using the `ios_config` module specifically for deploying global configuration commands.

> Note: The video does NOT cover sending commands in any other configuration mode such as interface configuration mode or router configuration mode.  I'd recommend simply starting with global configuration commands and expand from there exploring how to further use the module issuing the command `ansible-doc ios_config` on your Ansible machine.


-Jason

@jedelman8


