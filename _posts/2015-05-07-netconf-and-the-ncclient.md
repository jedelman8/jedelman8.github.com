---
layout: post
title: NETCONF and the ncclient
tags: netconf, ncclient, python
---

NETCONF is an industry standard (IETF) network management protocol.  It's actually been around for quite awhile and supported by numerous vendors.  While NETCONF is not always compatible across network switch platforms, it's the closest thing I can see that could be a unified multi-vendor API.  Of course, there are also vendor extensions for those device-specific features too.

I'm not going to get too much into what NETCONF is because Matt Oswalt has already done [that](http://keepingitclassless.net/2015/03/sdn-protocols-part-5-netconf/).  Check out his post if you haven't already done so.  There are also plenty of other good resources on NETCONF out there.

What I am going to focus on in this post is using Python to interact with NETCONF-enabled network switches. 

Let's get to it.

First, you'll want to install the *ncclient*. It is pretty much the de facto Python library to use when you need a NETCONF client to communicate with a NETCONF server, i.e. a network device.

```
sudo pip install ncclient
```

This will also install a few other required dependencies such as paramiko and lxml along with the client itself.

The next thing you are going to need is at least one switch (or device) that supports NETCONF.  In this post, I'm using two switches: one Nexus 9396 switch and one HP 5930 switch.

To follow along, you can create a Python script or just use the dynamic Python interpreter.  I'll be using the interpreter for one example and a script for another.

After you enter the interpreter, import the `manager` object from `ncclient`.  This is the most critical object that is used to create and manage connections to the network device.

```
edelman@dev:~/hp/testing$ python
Python 2.7.6 (default, Mar 22 2014, 22:59:56) 
[GCC 4.8.2] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 
>>> from ncclient import manager
>>> 
```

Now you are ready to define your connection.  

### NETCONF with HP

As you can see below, there is some basic login information along with a few other parameters that are needed to get a connection going.  Note the default port for NETCONF is 830 on many systems (but not on others!).  On certain systems, they usually allow you to change the port number as well.

Note: the command used to enable NETCONF over SSH on the HP switch is: `[HP5930-1]netconf ssh server enable`.  HP also supports NETCONF over SOAP if that tickles your fancy.

For more detail on the other parameters used, check out the ncclient [documentation](http://ncclient.readthedocs.org/en/latest/index.html).

```
>>> with manager.connect(host='hp1',
...                      port=830,
...                      username='***',
...                      password='***',
...                      hostkey_verify=False,
...                      allow_agent=False,
...                      look_for_keys=False
...                      ) as netconf_manager:
...     
```

In this post, we are going to retrieve the configured VLANs of each switch mentioned already.  In order to do this, we must understand the XML data structure that each device uses.  By the way, NETCONF uses XML :)

If we examine the official HP 5930 Network Management & Monitoring Configuration Guide, we can see the high level outline they provide.  The following text is verbatim out of the configuration guide.

```
# Copy the following text to the client to perform the get operation:
<?xml version="1.0" encoding="UTF-8"?>
<rpc message-id="100" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <getoperation>
        <filter>
            <top xmlns=" http://www.hp.com/netconf/data:1.0">
                Specify the module, submodule, table name, and column name
            </top>
        </filter>
    </getoperation>
</rpc>
```

Now the key is to realize, they don't assume you'll be using the *ncclient*, which will actually provide some of the high level structure for you already!

For completeness, let's look at an example provided by Cisco [here](http://www.cisco.com/c/en/us/td/docs/switches/datacenter/sw/nx-os/xml/user/guide/nxos_xml_interface/using.html#wp1083307) - their docs are always pretty good to reference no matter what.

```
X <?xml version="1.0"?>
R <nc:rpc message-id="1" xmlns:nc="urn:ietf:params:xml:ns:netconf:base:1.0"
R  xmlns="http://www.cisco.com/nxos:1.0:nfcli">
N  <nc:get>
N    <nc:filter type="subtree">
D      <show>
D        <xml>
D          <server>
D            <status/>
D          </server>
D        </xml>
D      </show>
N    </nc:filter>
N  </nc:get>
R </nc:rpc>]]>]]>
```

Each of the lines start with a letter.  These letters are to be referenced as follows:

* X —XML declaration
* R—RPC request tag
* N—NETCONF operation tags
* D—Device tags 

This is being shown to differentiate between the **Device Tags** and pretty much everything else.  We want to focus on the device tags when preparing and sending an XML data structure using the *ncclient* to a particular network device.

So, let's get back to it.

Since our goal is to retrieve the VLANs, we need to understand the XML schema needed for VLANs.  You will need docs for this and the one I have for HP documents VLANs as follows:

```
<VLAN>
    <VLANs>
        <VLANID>
            <ID></ID>
            <Description></Description>
            <Name></Name>
            <RouteIfIndex></RouteIfIndex>
            <UntaggedPortList></UntaggedPortList>
            <TaggedPortList></TaggedPortList>
        </VLANID>
    </VLANs>
</VLAN>
```

We want all of the VLANs with all other information per VLAN, so we can just use what is below as our *filter*:

```
<VLAN>
    <VLANs>
    </VLANs>
</VLAN>
```

This filter gets defined as a string in Python as shown below.  Here you can see I only maintained the **device tags** based on the HP documentation which includes "*top*" and the "*feature/table*" tags as defined for each specific feature.  If you correlate the Cisco and HP doc snippets from above, you'll be able to see this.

```
...     
...     vlans_filter = '''
...                    <top xmlns="http://www.hp.com/netconf/data:1.0">
...                        <VLAN>
...                            <VLANs>
...                            </VLANs>
...                        </VLAN>
...                    </top>
...                    '''
```

> **Notice, I'm still indented under the `with` statement while in the interpreter!**

Now that the filter is defined, we can now make a request to the device and get our NETCONF on.  To do this we use the `get` method of the object we created earlier.

```
...                    
...     data = netconf_manager.get(('subtree', vlans_filter))
... 
>>> print data
```

For more information on the filter parameters, you can check this out [here](http://ncclient.readthedocs.org/en/latest/manager.html#filter-params).  

By the way, you could have done `netconf_manager.get()` without any parameters to retrieve a lot more information about the device!  

You can now print `data` to see what was returned back.  Since I only had VLANs 1 and 100 on the device, this is what was printed for me:

```
<?xml version="1.0" encoding="UTF-8"?><nc:rpc-reply
xmlns:nc="urn:ietf:params:xml:ns:netconf:base:1.0" message-
id="urn:uuid:2dae08d8-f436-11e4-b02d-080027cee472"><nc:data>
<top xmlns="http://www.hp.com/netconf/data:1.0"><VLAN><VLANs>
<VLANID><ID>1</ID><Description>VLAN 0001</Description>
<Name>VLAN 0001</Name></VLANID><VLANID><ID>100</ID>
<Description>VLAN 0100</Description><Name>vlan_test</Name>
</VLANID></VLANs></VLAN></top></nc:data></nc:rpc-reply>

```

Of course, this isn't printed pretty by default, but now you have a solid well known data structure that can be easily parsed using an xml library.  Will leave that for another post.

### NETCONF with Nexus

To do the same thing on a Nexus switch (not just a 9000), it would be the same approach.  The following shows a script (not on the interpreter) that can be used to extract the same information from nearly all models of Nexus switches.

```
#!/usr/bin/env python

from ncclient import manager

def main():

    with manager.connect(host='n9396-1',
                         port=22,
                         username='***',
                         password='***',
                         hostkey_verify=False,
                         device_params={'name': 'nexus'},
                         allow_agent=False,
                         look_for_keys=False
                         ) as cisco_manager:

        vlans_filter = '''
                        <show xmlns="http://www.cisco.com/nxos:1.0">
                            <vlan>
                            </vlan>
                        </show>
                       '''

        cisco_vlans = cisco_manager.get(('subtree', vlans_filter))

        print cisco_vlans

if __name__ == '__main__':
    main()
```

You will notice a new parameter, `device_params`, was used when defining the connection.  Luckily *ncclient* does support multiple (NOT ALL) vendors when they are not using the *"standard"* - at least that is how I perceive it.  If anyone wants to comment further below, I'm all ears.

One other difference to note comparing the HP and Cisco is that Cisco is using port 22 by default (no other commands were used on the Cisco to enable NETCONF other than to enable the SSH server on the switch).

Stay tuned for more posts in the future on using the *ncclient*.

Thanks,
Jason

Twitter: @jedelman8





