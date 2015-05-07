---
layout: post
title: Nexus 3000, Python, Linux, and Open Switch Platforms
tags: nexus, python, automation, linux
---

This post shares some thoughts on some recent testing I’ve done with a Cisco Nexus 3000 and its built-in Python interpreter.  It also touches upon why open and programmable could benefit the community with some concrete examples.

The application that I have started to build is all about more efficiently and more easily managing devices programmatically without using the CLI.  You will see that the [Python APIs (methods, functions, etc.)](http://www.cisco.com/en/US/docs/switches/datacenter/nexus3000/sw/python/api/API_functions.html) are still fairly limited on the 3K, so I did have to use the “CLI” function to send commands from Python to the native Cisco NX-OS CLI.  Having access to Linux could have made it possible to modify the files needed instead.
There are only two components that I used for this testing.  They are (1) Cisco Nexus 3000 switch and (2) Ubuntu server –  running Sublime Text 2, SFTP via OpenSSH, and Python.

While the main intent is to more programmatically manage network devices (and make it easier), the basis for this testing comes from Linux and tools like Chef and Puppet.  While I don’t have much experience in this area, the general concept is that a Chef or Puppet agent is loaded onto a device, it abstracts details of device configurations and CLI be it native Linux or network CLI, and its head end will dictate, enforce, and ensure that the configuration is as desired.  This is what is called a declarative approach to managing systems and infrastructure.  The state of which the administrator desires is defined and then this state is enforce and maintained.  This happens with a head end device like a Puppet Master working with and communicating with Puppet Agents loaded on various types of devices.  As you will see, my scenario really doesn't have a head-end per se.

If you've ever done any work in Linux, you quickly realize it’s a pain in the you know what to manage the system because there is no single configuration file (unlike network devices).  It’s extremely simple to do a ‘show running-config’ on a network device to capture the current configuration.  Instead on Linux, you have to manage 10s and 100s of files.  The power of Puppet/Chef is massive – if I was a sysadmin, I don’t know how I would cope without such tools.

Coming back to the network --- I was wondering what if there wasn't a single configuration file for each network device?  In fact, there isn't on [Cumulus Linux](http://cumulusnetworks.com/product/overview/).  What if you wanted to declare the desired state in the network without having to know various vendors' CLI?  What if you wanted to give different network admins different types of access to making changes on the network device?  These are just some of the questions I was trying to answer while experimenting with Python on the Nexus 3000.

### Programming the Nexus 3000

First, the switch configuration would be defined in a collection of files.  In my example, two of those files are ‘hostname’ and ‘vlans.’  The contents of these files can be seen below.  Contents of other files aren't shown just to keep this blog from turning into a novel.

**FILE: hostname**

```
{
       HOSTNAME=MYSWITCH
}

FILE: vlans
#This file contains VLAN information to be applied
#using the config.py script
#Format is VLAN=VLAN_ID.VLAN_NAME
{
     VLAN=2.MY_NETWORK
     VLAN=4.HR
     VLAN=5.IT
     VLAN=10.SDN
     VLAN=20.HDN
     VLAN=21.Test1
     VLAN=22.Test2
     VLAN=23.Test3
}
```

These files can be created anywhere and then placed in a repository.  In my case, these files were placed on the SFTP server in a common directory.  The SFTP server is setup in such a way where the user is restricted to a single directory on the server that maps to their username.  This can come in handy restricting certain users from transferring files they shouldn't have access to.  Once the files are in the repository, they can be loaded onto the switch.  In my case, I used SFTP from the CLI to get the files onto the switch.  There is actually a built in ‘transfer’ function in Python, but it was not working – could have been a bug or a user error.  This would have been the ideal way since it could have been embedded in the script.   The files were placed in a new directory on the switch called ‘/pyconfigs/’ located in ‘bootflash:’ once they were transferred to the switch. 

Note: if this was a switch with a factory default configuration, the required files and scripts could have been deployed with Cisco Power on Auto Provisioning (POAP).

The next step is to create a software agent (agent sounds better than script, right? :)) by leveraging Python that reads the contents of the files and makes the appropriate changes to the switch.  Below is the code I created to do this.  It’s not too many of lines of code (read: not too much time invested) and you can pick and choose which configurations you wish to do this for.  To get the point across, I’m only showing hostname and VLANs.

Side note:  here we are reading a collection of files and manipulating the single configuration file on the Nexus switch.  If this was Cumulus Linux, it could still be a collection of files declaring the intent for the network configuration in an easy to read language, which then translates the desired intent to native Linux and manipulates the various files instead of the single config file.  You can also use one file instead of many declaring the intent – my choice here was to keep the files small and easy to read.
I could have cleaned up the logic and general code below, but figured I'd share where I'm at right now.  There is also no error checking - that would be required if deploying something like this in production. And apparently, my blogging platform doesn't allow code to be displayed like other platforms, so indents and formatting could be a bit off.

```python
#config.py
import logging
import os
import json
import readline
from cisco import *

def getHostname():
     hostname = "switch"

     if os.path.exists("/bootflash/pyconfigs/hostname"):
          namefile = open('/bootflash/pyconfigs/hostname', 'r+')
          for line in namefile:
               if (line.split("=")[0].strip() == "HOSTNAME"):
                    hostname = line.split("=")[1]
     else:
           print "Hostname file does not exist or cannot be opened"
     namefile.close()
     return hostname

def configVLANS():
     if os.path.exists("/bootflash/pyconfigs/vlans"):
          namefile = open("/bootflash/pyconfigs/vlans", 'r+')
          for line in namefile:
               if line.strip().startswith("#") == False:
                    if (line.split("=")[0].strip() == "VLAN"):
                         temp1 = line.split("=")
                         temp2 = temp1[1]
                         temp_line = temp2.split(".")
                         vlanID = temp_line[0]
                         vlanName = temp_line[1]
                         print vlanID + "**" + vlanName 
                         c1 = CLI ('config t')
                         c2 = CLI ('vlan ' + vlanID)
                         c3 = CLI ('name ' + vlanName)
                         c4 = CLI ('exit')
     else:
         "VLAN file does not exist or cannot be opened"

# MAIN
newHostname = getHostname()
if newHostname != "switch":
     c1 = CLI ('config t')
     c2 = CLI ('hostname ' + newHostname)
     c3 = CLI ('exit')
else:
     print "No new hostname applied"
configVLANS()

"""
if os.path.exists("/bootflash/pyconfigs/hostname"):
namefile = open('/bootflash/pyconfigs/hostname', 'r+')
print "File is Open for Reading"
"""
```

The python agent can then be transferred into the same directory where the other files are on the Nexus 3000 switch.  The script can be run by using the following command on the Cisco CLI: ‘python bootflash:/pyconfigs/config.py’

That command runs it once, but in this design, the goal was to mimic that of a configuration management platform, by maintaining persistence, and not letting the configuration drift as they sometimes do over time especially when many engineers have access to the same devices.  To accomplish this, my first thought was to leverage Linux bash to run the python agent every X sections/minutes to ensure the proper configuration is applied; this is required since I don’t have a head-end to manage the various Python agents.

However, the Nexus 3000 doesn't give direct access to Linux, so that option was out.  One way documented to trigger Python scripts is to use Embedded Event Manager (EEM) – after looking through the options, it did not seem possible to re-apply the configuration based on a timer using EEM on the Nexus 3000.  EEM seems to require a trigger event - at least on this switch.  After a quick search on the Google, I found the ‘scheduler’ feature on the Nexus platform.  By using scheduler, the python agent is now executed once per minute.  

Here is the scheduler configuration:

```
config terminal
 scheduler job name config1
python bootflash:/pyconfigs/config.py
end

config terminal
  scheduler schedule name config1
    time start 2014:01:10:16:48 repeat 1
    job name config1
end
```

I then manually changed the hostname, deleted the VLANs, and as I went about my business on the switch, the configuration went back to what it should have been.  It  worked as planned!  Pretty sweet.

Remember, this means if you wish to change the hostname of the switch, just update the hostname in the file.  If want to add a VLAN, just update/add the new VLAN in the file.  No need to use the CLI or whatever is native on the box…and one of the best parts is, almost any level *IT* engineer can make these file changes without having CCIE level knowledge.  It could even work across different vendors’ platforms – that would be the end goal.

### Access to Linux

As someone who has really only known the traditional network CLI, it was hard to understand why a network engineer may want access to Linux on one of these new “open” switch platforms such as Cumulus Linux, Arista (has given customers access to Linux for years),  and the Cisco Nexus 9000.  In this small petty example of a network application, having this capability would have been beneficial for at least three reasons:

* I wouldn't have had to search for a way to execute the python agent.  EEM and Scheduler are vendor specific ways to execute a script.  It would have been nice to leverage Linux here.
* The Cisco Python interpreter seemed to be more sensitive with indentations than when I was testing on the Ubuntu machine.  While this isn't a big deal, I needed to make changes to the python code.  As far as I know, I cannot do this on the 3K even with a basic text editor.  If it was native Linux, the changes could have been made quick and easy in a generic text editor.  The key is I could have done this on the switch.  Instead, I had to SFTP the software script every time I wanted to make a change.  
* There aren't many available Python functions yet.  While having pre-built functions streamlines things, the more open the switch, you can just interact with the native switch/Linux file system to get/mofidy the data you need right in the Python script.  Having direct access to the file system would be preferred vs. issuing CLI commands via the Python CLI function. 

Strive for open.  Strive for programmability.  Strive for Linux accessibility.

And as always, I'm interested in feedback and general thoughts on something like this.  How are you thinking about leveraging native Linux and scripting in your network?  Comment below or contact me directly.

Thanks,
Jason

Twitter: @jedelman8


