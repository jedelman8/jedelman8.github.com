---
layout: post
title: Programmatic Access to CLI Devices with TextFSM
tags: automation, ansible, textfsm
---
One of the harder things to do when it comes to network automation is work with the majority of the install base that exists out there. This is true even if we focus purely on data extraction, i.e. issuing `show` commands and getting the results in an automated fashion.  The reason for this is that most devices do not support returning structured data in formats such as JSON or XML, and this often times makes automation a non-starter for network engineers.  

Traditionally, SSH is used to connect to a network device, issue a command, and dump plain text results back to the user.  This leaves the user with the task of parsing through raw text and probably working with a library built for working with regular expressions, e.g. `re` for Python.  If you make it this far, you become an expert in using expressions like this: `([A-Z])\w+`.  And that's not even a hard one!  Regex party, anyone?  I'll pass.

## TextFSM to the Rescue

What if there was a way to simplify the process of getting structured data out of the raw text a network device responds with?  As luck would have it, there is definitely a better way.  We can thank Google for it.  And I thank [Simon Knight](https://twitter.com/eskaytwo) who made me aware of it several months ago (just getting around to it now though!) when I was working with [Autonetkit](http://autonetkit.org/).  So what is it?  It's [TextFSM](https://code.google.com/p/textfsm/wiki/TextFSMHowto).

TextFSM is a state machine (basically like a templating engine) that is purpose built to simplify working with regular expressions and getting structured data out of **traditional** network devices.  Nifty.

For the more formal introduction taken directly from the TextFSM website:

> **TextFSM, originally developed to allow programmatic access to information given by the output of CLI driven devices, such as network routers and switches, it can however be used for any such textual output.**

## How does it work?

It basically takes raw text and renders it with a TextFSM template to create structured data. 

### An Example

First, we'll take a look at the output of a `show vlan` on a Cisco switch:

```
N3K# show vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Eth1/1, Eth1/2, Eth1/3
                                                Eth1/5, Eth1/6, Eth1/7
2    VLAN0002                         active    Po100, Eth1/49, Eth1/50
3    VLAN0003                         active    Po100, Eth1/49, Eth1/50
4    VLAN0004                         active    Po100, Eth1/49, Eth1/50
5    VLAN0005                         active    Po100, Eth1/49, Eth1/50
6    VLAN0006                         active    Po100, Eth1/49, Eth1/50
7    VLAN0007                         active    Po100, Eth1/49, Eth1/50
8    VLAN0008                         active    Po100, Eth1/49, Eth1/50
9    VLAN0009                         active
10   test_segment                     active    Po100, Eth1/49, Eth1/50
11   VLAN0011                         active    Po100, Eth1/49, Eth1/50
12   VLAN0012                         active    Po100, Eth1/49, Eth1/50
13   VLAN0013                         active    Po100, Eth1/49, Eth1/50
14   VLAN0014                         active    Po100, Eth1/49, Eth1/50
15   VLAN0015                         active    Po100, Eth1/49, Eth1/50
16   VLAN0016                         active    Po100, Eth1/49, Eth1/50
17   VLAN0017                         active    Po100, Eth1/49, Eth1/50
18   VLAN0018                         active    Po100, Eth1/49, Eth1/50
19   VLAN0019                         active    Po100, Eth1/49, Eth1/50
99   native                           active    

VLAN Type  Vlan-mode
---- ----- ----------
1    enet  CE     
2    enet  CE     
3    enet  CE     
4    enet  CE     
5    enet  CE     
6    enet  CE     
7    enet  CE     
8    enet  CE     
9    enet  CE     
10   enet  CE     
11   enet  CE     
12   enet  CE     
13   enet  CE     
14   enet  CE     
15   enet  CE     
16   enet  CE     
17   enet  CE     
18   enet  CE     
19   enet  CE     
99   enet  CE     
Primary  Secondary  Type             Ports
-------  ---------  ---------------  -------------------------------------------

```

Next, we'll need to create a TextFSM based template that will define and represent the data we want to extract.  In this example, I want the `id`, `name`, and `status` for each VLAN. 

So, a template that gets this data would look like the following:

```
Value VLAN_ID (\d+)
Value NAME (\w+)
Value STATUS (\w+)

Start
  ^\d+\s+enet\s+CE
  ^${VLAN_ID}\s+${NAME}\s+${STATUS}\s+ -> Record

```

It's still pretty much impossible to get away from regular expressions, but they are much more manageable here.  First variables (or values) are defined.  They are `VLAN_ID`, `NAME`, and `STATUS`.  Next to each variable name is the regular expression that represents the value being extracted.  So, we can see here that `VLAN_ID` will be one or more digits.

> #### Note: If I wanted to, I could have been more granular with some of these regex's, but there was no need for this example.
 
Under the variables that are defined is where you then create the regular expression(s) that are used to evaluate each line of raw text.  When a line from the raw text matches the regex, the next line of text is then evaluated, and so on.  When there is a match, you can do nothing (as shown in the first regex above) or create a **Record** and store the data (as shown in the second regex above).  The reason for the first regex `^\d+\s+enet\s+CE` is this matches entries in the second VLAN table shown in the `show vlan` output.  I'm basically disregarding that one, not recording anything there, and then storing all records for the first table.  There is much more to TextFSM, so definitely take a look at the official docs if you are interested in learning more.

If we use the `textfsm.py` script that comes with the install, the usage looks like this:

```
$ python textfsm.py show_vlan.templ show_vlan.txt 

FSM Template:
Value VLAN_ID (\d+)
Value NAME (\w+)
Value STATUS (\w+)

Start
  ^\d+\s+enet\s+CE
  ^${VLAN_ID}\s+${NAME}\s+${STATUS}\s+ -> Record


FSM Table:
['VLAN_ID', 'NAME', 'STATUS']
['1', 'default', 'active']
['2', 'VLAN0002', 'active']
['3', 'VLAN0003', 'active']
['4', 'VLAN0004', 'active']
['5', 'VLAN0005', 'active']
['6', 'VLAN0006', 'active']
['7', 'VLAN0007', 'active']
['8', 'VLAN0008', 'active']
['10', 'test_segment', 'active']
['11', 'VLAN0011', 'active']
['12', 'VLAN0012', 'active']
['13', 'VLAN0013', 'active']
['14', 'VLAN0014', 'active']
['15', 'VLAN0015', 'active']
['16', 'VLAN0016', 'active']
['17', 'VLAN0017', 'active']
['18', 'VLAN0018', 'active']
['19', 'VLAN0019', 'active']
['99', 'native', 'active']
$ 
```

It first prints out your template and then the table.  As you can see, the table is a `list` per VLAN, or in other words, a **Record** as defined in the template.

After seeing this, I wanted to do three things.

1. Convert the individual lists to a list of dictionaries such that each existing row is it's own dictionary and the keys are equal to the column headers as currently shown.
2. Make it possible to use this approach by using real-time state data and not just local text stored in offline files
3. Integrate with Ansible to eliminate the user from having to deal with the list to dict conversion, etc. and this way, it'll just seem like structured data is being returned from every device!

## What was the result?

As it stands now, it is an Ansible module that I'm calling **netget**.  It supports several parameters, but the main ones are: `connection`, `command`, `file`, and `template`.  `template` is required and you would put the path to the TextFSM based template here.  `command` and `connection` go together as this would be the `show` command or equivalent and type of connection you will be making.  Today, it only supports SSH, but it's open to support other mechanisms like telnet and console connections in the future.  Last, `file` can be used instead of `command` if you want to just work with offline files rather than collect data in real-time from devices.

> **One major point about this type of approach is the user can create whatever type of template they want without creating new Ansible modules!  Just create a new template and get structured data back!**

### Another Example - Gathering neighbor data from a switch

Template:

```

Value HOSTNAME (\S+)
Value LOCAL_INTF (\S+\s*\S+)
Value PLATFORM (\S{4,})
Value REMOTE_INTF (\S+\s*\S+)

Start
  ^Device-*\s*I[Dd] -> TABLE

TABLE
  ^${HOSTNAME}\s+${LOCAL_INTF}\s+\d+[\w\s]+\s+${PLATFORM}\s+${REMOTE_INTF} -> Record
  ^\w+#
  ^${HOSTNAME}
  ^\s+${LOCAL_INTF}\s+\d+[\w\s]+\s+${PLATFORM}\s+${REMOTE_INTF} -> Record

```

This one is a little more complex.  I'll let you evaluate it :)

Running an Ansible playbook using **netget**:

```
$ ansible-playbook getmemydata.yml -v

PLAY [n9k1] ******************************************************************* 

TASK: [netget connection=ssh template=/home/cisco/projects/legacy/show_cdp_neigh.tmpl command='show cdp neighbors' host={{ inventory_hostname }} username=cisco password='!cisco123!'] *** 
ok: [n9k1] => {"changed": false, "sio": [{"hostname": "c3550", "local_intf": "mgmt0", "platform": "WS-C3550-24", "remote_intf": "Fas0/22"}, {"hostname": "N9K2.cisconxapi.com(SAL1819S6LU)", "local_intf": "Eth1/2", "platform": "N9K-C9396PX", "remote_intf": "Eth1/2"}, {"hostname": "N9K2.cisconxapi.com(SAL1819S6LU)", "local_intf": "Eth2/12", "platform": "N9K-C9396PX", "remote_intf": "Eth2/12"}]}

PLAY RECAP ******************************************************************** 
n9k1                       : ok=1    changed=0    unreachable=0    failed=0   

cisco@onepk:~/projects/legacy$ 
```

So there you have it!  A way to programmatically get structured data out of CLI driven systems.  Not too shabby.

Almost forgot - here is the Playbook:

```
---
- hosts: n9k1
  connection: local
  gather_facts: False

  tasks:

    - netget:
        connection=ssh
        template=/home/cisco/projects/legacy/show_cdp_neigh.tmpl
        command='show cdp neighbors'
        host={ { inventory_hostname } }
        username=cisco
        password='!cisco123!'

```

>  **Note: Double curly braces and what is inside them are not being shown when this page is being converted from markdown to html, so an extra space has been added for visual representation only in the playbook for the host param.**

Since I am not regex guru, I've come to use the following site quite a bit to test and troubleshoot regular expressions: [http://www.regexr.com/](http://www.regexr.com/).  There are plenty of others, but this one has been doing the trick for me.

Side note:  I haven't posted the code yet for the **netget** module mainly because it doesn't have all the functionality I'd like it to have yet. And I started using Paramiko as the library, but think it probably makes more sense to use the library, called **netmiko**, that [Kirk Byers](https://twitter.com/kirkbyers) has been building since it's multi vendor, etc., which also depends on Paramiko anyway.

Given that the industry has kind of standardized on a common CLI, this method should even work across multiple vendors, which is pretty damn cool.  TextFSM is definitely not limited to network gear though, so feel free to use it on whatever device you want - even those native Linux operating systems :)

What do you think?  What other functionality would you like to see in something like this?  


Thanks,
Jason

Twitter: @jedelman8


