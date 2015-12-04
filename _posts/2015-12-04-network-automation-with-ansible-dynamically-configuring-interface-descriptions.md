---
layout: post
tags: ansible, cisco, arista, automation
title: Network Automation with Ansible - Dynamically Configuring Interface Descriptions
---

It's been a while since my last post, but let's hope that changes with the flurry of posts planned for this month.  Most of my recent time has been spent traveling and teaching [courses](http://networktocode.com/training/course-schedule/) that cover how to use Python and Ansible for Network Automation.  I've written about many of these concepts in the past, but to re-iterate what I've been saying, and what I've written in the past, it's crucial to start small when it comes to automation (otherwise it's easy to feel overwhelmed trying to automate *everything* and then you never make any real progress).  By starting small, you can get a quick win, and can gradually expand from there.  In this post, I'm going to review one very small example of how to use Ansible for network automation.  We'll review how to use Ansible to dynamically configure interface descriptions populated with real-time LLDP neighbor information.  While this post focuses on Cisco Nexus switches, note that the same approach can be used for any vendor.

The process that we'll be using to auto-configure the interface descriptions is a three-step process:

**1. Discover the device**
While we are only using Cisco switches in this example, we still go through the discovery process knowing *ONLY* the SNMP read-only community string of the device.  Using the Ansible module called **snmp_device_version** we discover the vendor, OS, and OS version of the device. 

**2. Discover the LLDP neighbors of the device**
The neighbors are required because they'll be used to populate the interface descriptions we configure in a subsequent task.  Using the Ansible module called [**ntc_show_command**](/home/creating-templates-for-textfsm-and-ntc_show_command), the neighbors are gathered from the device and *registered*, so we can access them later on in the playbook.

**3. Configure the interface descriptions**
We use the **nxos_interface** module to configure the interface descriptions using the data *registered* from the previous task.

These three steps map directly to the three tasks used in the Ansible playbook in this post.

Let's dive in and take a look at the playbook and break down each task in more detail.

> Note that the playbook has the filename `auto-config-port-descriptions.yml`.

{% raw %}
```yaml
---

  - name: AUTO CONFIGURE PORT DESCRIPTIONS
    hosts: cisco
    gather_facts: no
    connection: local

    tasks:

      - name: GET SNMP DISCOVERY INFORMATION
        snmp_device_version: host={{ inventory_hostname }} community=networktocode version=2c
        tags:
          - snmp
          - neighbors
 
      - name: GET LLDP NEIGHBORS
        ntc_show_command:
          connection=ssh
          platform={{ ansible_device_vendor }}_{{ ansible_device_os }}
          template_dir='/home/ntc/library/ntc-ansible/ntc_templates'
          command='show lldp neighbors'
          host={{ inventory_hostname }}
          username={{ un }}
          password={{ pwd }}
        register: neighbors
        tags: neighbors

      - name: CONFIGURE PORT DESCRIPTIONS USING NEIGHBOR DATA
        nxos_interface:
          interface={{ item.local_interface  }}
          description="Connects to {{ item.neighbor_interface }} on {{ item.neighbor }}"
          host={{ inventory_hostname }}
          username={{ un }}
          password={{ pwd}}
        with_items: neighbors.response
```
{% endraw %}

The first task uses SNMP to collect the device vendor, OS, and OS version from the device.  We're also using tags so we can selectively run this task without running the complete playbook.

Let's run *only* the first task using the tag **snmp**.

```
ntc@ntc:~$ ansible-playbook auto-config-port-descriptions.yml --tags=snmp

PLAY [AUTO CONFIGURE PORT DESCRIPTIONS] *************************************** 

TASK: [GET SNMP DISCOVERY INFORMATION] **************************************** 
ok: [nx1]
ok: [nx2]

PLAY RECAP ******************************************************************** 
nx1                        : ok=1    changed=0    unreachable=0    failed=0   
nx2                        : ok=1    changed=0    unreachable=0    failed=0   

ntc@ntc:~$ 
```

As you can see, it runs successfully.  But, where is the device information collected via SNMP?  To see what data is being collected (gathered from the device) we can simply run the playbook in verbose mode using the `-v` flag.

> Note: in this particular case, we don't actually need to use **register** like we do in the next task because the data is returned using the `ansible_facts` key.  Anything that is a **fact** is always accessible without using **register**.

Let's run the playbook again, but this time in verbose mode.

```
ntc@ntc:~$ ansible-playbook auto-config-port-descriptions.yml --tags=snmp -v

PLAY [AUTO CONFIGURE PORT DESCRIPTIONS] *************************************** 

TASK: [GET SNMP DISCOVERY INFORMATION] **************************************** 
ok: [nx1] => {"ansible_facts": {"ansible_device_os": "nxos", "ansible_device_vendor": "cisco", "ansible_device_version": "7.1(0)D1(1)"}, "changed": false}
ok: [nx2] => {"ansible_facts": {"ansible_device_os": "nxos", "ansible_device_vendor": "cisco", "ansible_device_version": "7.1(0)D1(1)"}, "changed": false}

PLAY RECAP ******************************************************************** 
nx1                        : ok=1    changed=0    unreachable=0    failed=0   
nx2                        : ok=1    changed=0    unreachable=0    failed=0   

ntc@ntc:~$ 
```

As you can see, three key-value pairs were returned for each device (nx1 and nx2):

  * "ansible_device_os": "nxos"
  * "ansible_device_vendor": "cisco"
  * "ansible_device_version": "7.1(0)D1(1)"

We now know exactly what types of devices these are!

The second task is the following:

{% raw %}
```
      - name: GET LLDP NEIGHBORS
        ntc_show_command:
          connection=ssh
          platform={{ ansible_device_vendor }}_{{ ansible_device_os }}
          template_dir='/home/ntc/library/ntc-ansible/ntc_templates'
          command='show lldp neighbors'
          host={{ inventory_hostname }}
          username={{ un }}
          password={{ pwd }}
        register: neighbors
        tags: neighbors
```
{% endraw %}

This task uses the [**ntc_show_command**](/home/creating-templates-for-textfsm-and-ntc_show_command) module.  This module uses SSH to connect to the device, then captures the raw text output from the `show lldp neighbors` command, and finally renders that text with a [TextFSM](/home/getting-structured-data-back-from-cli-devices) template (this all happens behind the scenes!).  It results in structured data (JSON) being returned that can be *registered* and used in the playbook!

The **ntc_show_command** module takes a few inputs with one of them being `platform`.  When using Nexus, the `platform` needs to be set to "cisco_nexus".  Since we're only automating Nexus switches for this playbook, we could have hard-coded "cisco_nexus" in the playbook, but instead, we used the data returned from the first task.  This is shown in this line: {% raw %} `platform={{ ansible_device_vendor }}_{{ ansible_device_os }}` {% endraw %}.  Note that you use curly braces to access a variable in Ansible.  Using variables makes this more dynamic, and now we can run the same command (using the same task) on Arista devices (for example) without changing anything!

Let's run the playbook again, now using the **neighbors** tag.  This runs task 1 and task 2 since they each have the **neighbors** tag.

```
ntc@ntc:~$ ansible-playbook auto-config-port-descriptions.yml --tags=neighbors

PLAY [AUTO CONFIGURE PORT DESCRIPTIONS] *************************************** 

TASK: [GET SNMP DISCOVERY INFORMATION] **************************************** 
ok: [nx1]
ok: [nx2]

TASK: [GET LLDP NEIGHBORS] **************************************************** 
ok: [nx2]
ok: [nx1]

PLAY RECAP ******************************************************************** 
nx1                        : ok=2    changed=0    unreachable=0    failed=0   
nx2                        : ok=2    changed=0    unreachable=0    failed=0   

ntc@ntc:~$ 
```

It runs successfully, but again, what data is being returned for the second task?

Remember, we can run the playbook in verbose mode to find out.

```
ntc@ntc:~$ ansible-playbook auto-config-port-descriptions.yml --tags=neighbors -v

PLAY [AUTO CONFIGURE PORT DESCRIPTIONS] *************************************** 

TASK: [GET SNMP DISCOVERY INFORMATION] **************************************** 
ok: [nx2] => {"ansible_facts": {"ansible_device_os": "nxos", "ansible_device_vendor": "cisco", "ansible_device_version": "7.1(0)D1(1)"}, "changed": false}
ok: [nx1] => {"ansible_facts": {"ansible_device_os": "nxos", "ansible_device_vendor": "cisco", "ansible_device_version": "7.1(0)D1(1)"}, "changed": false}

TASK: [GET LLDP NEIGHBORS] **************************************************** 
ok: [nx1] => {"changed": false, "response": [{"local_interface": "Eth2/1", "neighbor": "nx2", "neighbor_interface": "Eth2/1"}, {"local_interface": "Eth2/2", "neighbor": "nx2", "neighbor_interface": "Eth2/2"}, {"local_interface": "Eth2/3", "neighbor": "nx2", "neighbor_interface": "Eth2/3"}]}
ok: [nx2] => {"changed": false, "response": [{"local_interface": "Eth2/1", "neighbor": "nx1", "neighbor_interface": "Eth2/1"}, {"local_interface": "Eth2/2", "neighbor": "nx1", "neighbor_interface": "Eth2/2"}, {"local_interface": "Eth2/3", "neighbor": "nx1", "neighbor_interface": "Eth2/3"}]}

PLAY RECAP ******************************************************************** 
nx1                        : ok=2    changed=0    unreachable=0    failed=0   
nx2                        : ok=2    changed=0    unreachable=0    failed=0   

ntc@ntc:~$ 
```

You can see that a list of key-value pairs (or dictionaries) is returned for each device.  Each neighbor is a dictionary that has 3 keys or attributes of the neighbor, namely `local_interface`, `neighbor_interface`, and `neighbor`.  

Using **register** in the second task is what allows us to use this data elsewhere in the playbook (or template).  We are basically saying, save this JSON object as the variable called `neighbors` with the statement `register: neighbors`.

Finally, let's look at the third task uses this data to auto-configure the interface descriptions.

{% raw %}
```
      - name: CONFIGURE PORT DESCRIPTIONS USING NEIGHBOR DATA
        nxos_interface:
          interface={{ item.local_interface  }}
          description="Connects to {{ item.neighbor_interface }} on {{ item.neighbor }}"
          host={{ inventory_hostname }}
          username={{ un }}
          password={{ pwd}}
        with_items: neighbors.response
```
{% endraw %}

The third tasks iterates (loops) through the list that was stored as `response` from the data registered in the second task.  

To clarify, `neighbors.response` is a list, which now has the following value assigned to it (showing the list just for nx1):

```
[{"local_interface": "Eth2/1", "neighbor": "nx2", "neighbor_interface": "Eth2/1"},
{"local_interface": "Eth2/2", "neighbor": "nx2", "neighbor_interface": "Eth2/2"},
{"local_interface": "Eth2/3", "neighbor": "nx2", "neighbor_interface": "Eth2/3"}]
```

This tells us that "nx1" has three neighbors, one found on Eth2/1, Eth2/2, and Eth2/3.

But remember, this is a **list of dictionaries**.  This means during each iteration using **with_items**, the dictionary is accessed as `item` and the respective value is accessed using `item.key`.  We need to use curly braces again because they are variables.  By now, you can probably figure out what the descriptions are going to be after they are pushed to the devices.

It's worth pointing out that this third task is using the module called **nxos_interface** that uses NX-API on the back-end, to manage interface attributes such as speed, duplex, admin state, and descriptions on interfaces.

Let's run the complete playbook.

```
ntc@ntc:~$ ansible-playbook auto-config-port-descriptions.yml

PLAY [AUTO CONFIGURE PORT DESCRIPTIONS] *************************************** 

TASK: [GET SNMP DISCOVERY INFORMATION] **************************************** 
ok: [nx2]
ok: [nx1]

TASK: [GET LLDP NEIGHBORS] **************************************************** 
ok: [nx1]
ok: [nx2]

TASK: [CONFIGURE PORT DESCRIPTIONS USING NEIGHBOR DATA] *********************** 
changed: [nx1] => (item={u'neighbor_interface': u'Eth2/1', u'local_interface': u'Eth2/1', u'neighbor': u'nx2'})
changed: [nx2] => (item={u'neighbor_interface': u'Eth2/1', u'local_interface': u'Eth2/1', u'neighbor': u'nx1'})
changed: [nx2] => (item={u'neighbor_interface': u'Eth2/2', u'local_interface': u'Eth2/2', u'neighbor': u'nx1'})
changed: [nx1] => (item={u'neighbor_interface': u'Eth2/2', u'local_interface': u'Eth2/2', u'neighbor': u'nx2'})
changed: [nx2] => (item={u'neighbor_interface': u'Eth2/3', u'local_interface': u'Eth2/3', u'neighbor': u'nx1'})
changed: [nx1] => (item={u'neighbor_interface': u'Eth2/3', u'local_interface': u'Eth2/3', u'neighbor': u'nx2'})

PLAY RECAP ******************************************************************** 
nx1                        : ok=3    changed=1    unreachable=0    failed=0   
nx2                        : ok=3    changed=1    unreachable=0    failed=0   

ntc@ntc:~$ 
```

This successfully queried the device using SNMP, gathered LLDP neighbors in real-time, and configured interface descriptions on every interface that has a neighbor.  

Here is a snippet from the "nx1" device that shows the descriptions configured following the playbook being executed.

```
interface Ethernet2/1
  description Connects to Eth2/1 on nx2
  switchport
  no shutdown

interface Ethernet2/2
  description Connects to Eth2/2 on nx2
  switchport
  no shutdown

interface Ethernet2/3
  description Connects to Eth2/3 on nx2
  switchport
  no shutdown

interface Ethernet2/4
  shutdown
  no switchport
  mac-address 000c.293a.e20c
```


For completeness, this is the inventory file that was used along with the playbook:

```
[cisco:vars]
un=cisco
pwd=cisco

[cisco]
nx1
nx2
```

The devices "nx1" and "nx2" were mapped to their respective IP addresses in the `/etc/hosts` file where the playbook was run (DNS entries would have worked fine too).

This nice thing about this form of automation is that it is NOT anything fancy at all, but it truly helps day to day ops and can be used for multi-vendor environments.  

In addition, these modules prove that Ansible works great with ANY type of programmatic interface (or lack thereof):

* **snmp_device_version** uses SNMP to query devices
* **ntc_show_command** uses SSH to connect and collect data from devices
* **nxos_interface** uses the Nexus NX-API to communicate to Nexus switches

Big shout out to Patrick Ogenstad (@networklore) who developed the **snmp_device_version** module and to Kirk Byers (@kirkbyers) who developed netmiko, which is what **ntc_show_command** uses for transport.

For more detail on the modules mentioned in this post, check out their respective GitHub pages:

* [snmp_device_version](https://github.com/networklore/ansible-snmp/tree/master/library)
* [ntc_show_command](https://github.com/networktocode/ntc-ansible)
* [nxos_interface](https://github.com/jedelman8/nxos-ansible/blob/master/docs/nexus-module-docs.md#nxos_interface)

So how about all that?  What do you think? Not too shabby if you ask me.  

What do you wish you can automate?  Write in below!


Thanks,
Jason

Twitter: @jedelman8

