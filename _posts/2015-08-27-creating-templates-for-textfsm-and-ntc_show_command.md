---
layout: post
tags: textfsm, automation, ansible
title: Creating Templates for TextFSM and ntc_show_command
---

Less than two weeks ago I wrote a [post](/home/getting-structured-data-back-from-cli-devices) about an Ansible module called **[ntc_show_command](https://github.com/networktocode/ntc-ansible)**.  For those that didn't read that post, you should, but ntc_show_command is a multi-vendor module that can automate converting raw text from show commands into structured data, namely JSON.  

We've already had several pull requests enhancing the architecture, so the community support is off to a great start!  But in order to really make an impact, we (me, you, and fellow network engineers) need to continue to contribute templates to the project repository.  Templates are key to converting the raw text into JSON.

This post will walk through how to create a template for two different commands.  We'll take a look at *show version* for Cisco NX-OS and *display version* for HP Comware 7.  

The first thing that we'll need to do is get the raw text output that we want to *JSONify*.  We'll start with *show version*.

Below is the sample output that we'll work with and this file will be saved as `tests/cisco_nxos/cisco_nxos_show_version.raw` within our project directory.

```
Cisco Nexus Operating System (NX-OS) Software
TAC support: http://www.cisco.com/tac
Copyright (C) 2002-2014, Cisco and/or its affiliates.
All rights reserved.
The copyrights to certain works contained in this software are
owned by other third parties and used and distributed under their own
licenses, such as open source.  This software is provided "as is," and unless
otherwise stated, there is no warranty, express or implied, including but not
limited to warranties of merchantability and fitness for a particular purpose.
Certain components of this software are licensed under
the GNU General Public License (GPL) version 2.0 or 
GNU General Public License (GPL) version 3.0  or the GNU
Lesser General Public License (LGPL) Version 2.1 or 
Lesser General Public License (LGPL) Version 2.0. 
A copy of each such license is available at
http://www.opensource.org/licenses/gpl-2.0.php and
http://opensource.org/licenses/gpl-3.0.html and
http://www.opensource.org/licenses/lgpl-2.1.php and
http://www.gnu.org/licenses/old-licenses/library.txt.

Software
  BIOS: version 07.15
  NXOS: version 6.1(2)I3(1)
  BIOS compile time:  06/29/2014
  NXOS image file is: bootflash:///n9000-dk9.6.1.2.I3.1.bin
  NXOS compile time:  9/27/2014 23:00:00 [09/28/2014 06:23:37]


Hardware
  cisco Nexus9000 C9396PX Chassis 
  Intel(R) Core(TM) i3-3227U C with 16402544 kB of memory.
  Processor Board ID SAL1819S6LU

  Device name: N9K1
  bootflash:   21693714 kB
Kernel uptime is 123 day(s), 5 hour(s), 15 minute(s), 19 second(s)

Last reset 
  Reason: Unknown
  System version: 6.1(2)I3(1)
  Service: 

plugin
  Core Plugin, Ethernet Plugin

Active Packages:

```

We can get crazy extracting every value, but for now, let's assume the requirements are to extract the following:

  * uptime
  * last_reboot_reason
  * os
  * boot_image
  * platform

I'm not sure if anyone really likes working with regular expressions, but using TextFSM and templates simplifies the process quite a bit!  We now need to determine the RegEx for each of the values we need to extract.

In order to help, we'll use [http://regexr.com/](http://regexr.com/).

Let's start with *uptime*.

We'll enter the full line of text where the uptime is stored into the text area as shown in the picture below.

![p1](/img/p1.PNG)

After doing that, we need to determine a regex that can be used to extract the uptime.  For this post, we are going to keep it simple in that we will not extract days, hours, minutes, and seconds as separate values, but as a single string.  I encourage you to use the *Cheatsheet* on the left pane of the website.

Our goal is to ensure the full line of "text" turns blue.  This helps validate the regular expression.

The one that I came up with for this post is `Kernel uptime is (\d+\s\w+.s.,?\s?){4}`.  Feel free to update or modify this as you see fit (or issue a [PR](https://github.com/networktocode/ntc-ansible) :)).

![p2](/img/p2.PNG)

The next step will be to create a very basic TextFSM template that uses this regex.

The template will be stored as `ntc_templates/cisco_nxos_show_version.template` and it looks like this:

```
Value UPTIME ((\d+\s\w+.s.,?\s?){4})

Start
  ^Kernel uptime is\s${UPTIME} -> Record

```

The first part of the template is saying **UPTIME** is a variable (or value) we will be extracting from the raw text, and the regex for **UPTIME** is wrapped in parentheses, which is `((\d+\s\w+.s.,?\s?){4})`.  Not too bad, right?

Proper syntax in TextFSM requires a space and then *Start*.  Then indent two spaces, and insert the regex that was developed.  If there is a match, the value will be *recorded*.

Now it's time to test this out.

We'll run a quick test using a playbook called `je-test.yml`:

{% raw %}
```
---

- name: GET STRUCTURED DATA BACK FROM CLI DEVICES
  hosts: n9396-1
  connection: local
  gather_facts: False

  tasks:

    - name: test
      ntc_show_command:
        connection: offline
        file: tests/cisco_nxos/cisco_nxos_show_version.raw
        vendor: cisco_nxos
        device_type: cisco_nxos
        command: 'show version'
        host: "{{ inventory_hostname }}"
        username: "{{ username }}"
        password: "{{ password }}"
```
{% endraw %}

> Note: parameters will be updated soon such that vendor and device_type are not required.  Going forward, only device_type will be required.  Check the GitHub repo to stay up to date!

If we run the playbook, as shown below, we see that we are getting a list of dictionaries back.  This is the way the module works, so with commands like 'show version', it'll always be a list of one.  For neighbors, vlans, interfaces, and other objects when there are multiples, it'll be a list of N dictionaries, but please note it also depends on how the template is architected and when objects are *recorded*.

```
$ ansible-playbook -i hosts je-test.yml -v

PLAY [GET STRUCTURED DATA BACK FROM CLI DEVICES] ****************************** 

TASK: [test] ****************************************************************** 
ok: [n9396-1] => {"changed": false, "response": [{"uptime": "123 day(s), 5 hour(s), 15 minute(s), 19 second(s)"}]}

PLAY RECAP ******************************************************************** 
n9396-1                    : ok=1    changed=0    unreachable=0    failed=0   

```

Let's update the template with the other four values we want to extract.

The new template looks like this:

```
Value UPTIME ((\d+\s\w+.s.,?\s?){4})
Value LAST_REBOOT_REASON (\w+)
Value OS (\d+.\d+(.+)?)
Value BOOT_IMAGE (.*)
Value PLATFORM (\w+)

Start
  ^\s+NXOS: version\s${OS}
  ^\s+NXOS image file is:\s${BOOT_IMAGE}
  ^\s+cisco Nexus\d+\s${PLATFORM}
  ^Kernel uptime is\s${UPTIME}
  ^\s+Reason:\s${LAST_REBOOT_REASON} -> Record

```

**NOTE:** these Regex's can use improvement for sure.  The point is to show that *simple* regex's can be constructed and work.  Issue a PR to the project if you want to enhance them.  They've also only been tested on a Nexus 9396 and it would be good to test on other Nexus platforms too.  Any takers?

And here is an updated playbook:

{% raw %}
```
---

- name: GET STRUCTURED DATA BACK FROM CLI DEVICES
  hosts: n9396-1
  connection: local
  gather_facts: False

  tasks:

    - name: GET DATA
      ntc_show_command:
        connection: offline
        file: tests/cisco_nxos/cisco_nxos_show_version.raw
        vendor: cisco_nxos
        device_type: cisco_nxos
        command: 'show version'
        host: "{{ inventory_hostname }}"
        username: "{{ username }}"
        password: "{{ password }}"
      register:  mydata

    - name: DISPLAY BOOT IMAGE
      debug: msg="Boot image is {{ mydata.response[0].boot_image }}"

    - name: DISPLAY UPTIME
      debug: msg="Uptime is {{ mydata.response[0].uptime }}"
```
{% endraw %}

Running this playbook yields the following:

```
ansible-playbook -i hosts je-test.yml 

PLAY [GET STRUCTURED DATA BACK FROM CLI DEVICES] ****************************** 

TASK: [GET DATA] ************************************************************** 
ok: [n9396-1]

TASK: [DISPLAY BOOT IMAGE] **************************************************** 
ok: [n9396-1] => {
    "msg": "Boot image is bootflash:///n9000-dk9.6.1.2.I3.1.bin"
}

TASK: [DISPLAY UPTIME] ******************************************************** 
ok: [n9396-1] => {
    "msg": "Uptime is 123 day(s), 5 hour(s), 15 minute(s), 19 second(s)"
}

PLAY RECAP ******************************************************************** 
n9396-1                    : ok=3    changed=0    unreachable=0    failed=0  
```


The final test will be to ensure this works when getting data in real-time from the device.  The final playbook will use `ssh` instead of `offline` for the connection parameter.

{% raw %}
```
  tasks:

    - name: GET DATA
      ntc_show_command:
        connection: ssh
        vendor: cisco_nxos
        device_type: cisco_nxos
        command: 'show version'
        host: "{{ inventory_hostname }}"
        username: "{{ username }}"
        password: "{{ password }}"
      register:  mydata

    - name: DISPLAY BOOT IMAGE
      debug: msg="Boot image is {{ mydata.response[0].boot_image }}"

    - name: DISPLAY UPTIME
      debug: msg="Uptime is {{ mydata.response[0].uptime }}"
```
{% endraw %}

And below you can see the uptime is more than two days greater than the previous examples - fortunately, I was able to find the time to finish this post (two days after starting!) :)

```
ansible-playbook -i hosts je-test.yml -v

PLAY [GET STRUCTURED DATA BACK FROM CLI DEVICES] ****************************** 

TASK: [GET DATA] ************************************************************** 
ok: [n9396-1] => {"changed": false, "response": [{"boot_image": "bootflash:///n9000-dk9.6.1.2.I3.1.bin", "last_reboot_reason": "Unknown", "os": "6.1(2)I3(1)", "platform": "C9396PX", "uptime": "125 day(s), 21 hour(s), 3 minute(s), 50 second(s)"}]}

TASK: [DISPLAY BOOT IMAGE] **************************************************** 
ok: [n9396-1] => {
    "msg": "Boot image is bootflash:///n9000-dk9.6.1.2.I3.1.bin"
}

TASK: [DISPLAY UPTIME] ******************************************************** 
ok: [n9396-1] => {
    "msg": "Uptime is 125 day(s), 21 hour(s), 3 minute(s), 50 second(s)"
}

PLAY RECAP ******************************************************************** 
n9396-1                    : ok=3    changed=0    unreachable=0    failed=0   

```


The post was getting longer than anticipated so future posts will show how to create the template for the HP switch, and also how to create a proper test.

Any questions, let me know!


Thanks,
Jason

Twitter: @jedelman8

