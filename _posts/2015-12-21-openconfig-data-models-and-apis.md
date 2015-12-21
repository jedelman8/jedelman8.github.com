---
layout: post
tags: cisco, arista, openconfig
title: OpenConfig, Data Models, and APIs
---

_[Special thanks to [Rob Shakir](https://li.linkedin.com/in/robjs) for taking the time to talk about OpenConfig and the related work he has going on.  He definitely helped make the second half of this post happen- thank you, Rob.  Note: all of the BGP code examples are borrowed from Rob and his original work can be found [here](http://rob.sh/post/209).]_

# Introduction

As more devices continue to add support for APIs, and the industry migrates from [CLI to API](/home/from-cli-to-api-at-networking-field-day), the question often arises, "is there ever going to be a common multi-vendor network device API?"

Let me answer that for you, *"No!"*  Why?  Think about it.  What's in it for the vendors?  

> If you keep reading, you may see that there is in fact a reason for vendors to develop a common API. 

That said, this is the reason I initiated [CPAL](/home/common-programmable-abstraction-layer/) almost 2 years ago, which didn't go anywhere for a number of reasons, and as an aside, we are re-visiting the idea beyond CPAL, and you should see something within a few weeks!  And this is also the reason we have projects such as [netmiko](https://github.com/ktbyers/netmiko), [ntc-ansible](https://github.com/networktocode/ntc-ansible), [NAPALM](https://github.com/spotify/napalm), and one that is the focus of this post, [OpenConfig](http://www.openconfig.net/).

This post provides an introduction to OpenConfig and a perspective on what the OpenConfig group is working on.  Additionally, we'll dive into APIs and data models, looking at what they are, and why both are important.

# What is OpenConfig?

> *OpenConfig is an informal working group of network operators sharing the goal of moving our networks toward a more dynamic, programmable infrastructure by adopting software-defined networking principles such as declarative configuration and model-driven management and operations* - [http://www.openconfig.net/](http://www.openconfig.net/)

That sounds pretty awesome, but since modern network devices already have programmatic interfaces (such as NETCONF, REST, etc.), aren't we already moving networks toward a more programmable infrastructure?  Absolutely.  But, let's keep reading...

> *The initial focus of the effort is on the development of vendor-neutral data models for configuration and management that will be supported natively on networking hardware and software platforms.* - [http://www.openconfig.net/](http://www.openconfig.net/)

And from the OpenConfig [FAQ](http://www.openconfig.net/documents/faq) page:

>*Key focus areas being addressed by OpenConfig include development of vendor-neutral APIs....*

In summary, it appears OpenConfig is in fact focused on developing a multi-vendor network device API that uses vendor-neutral data models.  Fair?  Sure.  At least, that is what I thought going into a recent [Tech Field Day Event](http://techfieldday.com/event/dfdr1/).

As you can see [first hand](http://techfieldday.com/appearance/anees-shaikh-presents-open-network-management/), I asked Anees Shaikh (Google) some specific questions about the OpenConfig working group, specifically around whether or not the group is focused on APIs or data models.  As you'll see if you watch the [video](http://techfieldday.com/appearance/anees-shaikh-presents-open-network-management/), it's all about data models.

What is a data model anyway?  Isn't having a uniform API what we really want?  

Not so much.  Let's see why.

# APIs, Transport, and Data Models

If we think about APIs, we commonly think about transport and forget about something much more important --- which is how the data is modeled and sent across the wire. We need to remember APIs are NOT just about transport, so asking if a vendor just supports this or that type of transport is not enough (but it's all we have for now).

Modern SDN controllers have REST APIs, Juniper supports NETCONF, HP supports NETCONF on their Comware7 platforms, Brocade supports NETCONF, Cisco supports NETCONF on a few different platforms, and then they have a REST interface on the Nexus platform, and the same goes for Arista.  And as we know, all devices support SSH/CLI and SNMP.

If we take devices that use the same type of transport (it can be any of the few mentioned already) those devices still represent data differently.  This is most easily seen on *legacy* devices by the output of CLI commands.  While SSH/CLI commands return raw text and REST APIs may return data in JSON, the data is **NOT** represented the same across vendors.  For CLI output, this means there is no standard for displaying the text (and definitely no structure of that data) and for JSON, this means there is no standard for the key-value pairs being used.  We can see that using a common transport type or data format like JSON doesn't help all that much when actually *using* that data.

As we build on this, we can see less importance on the underlying transport (*I don't care how you get me the data*), and more importance on how the data is represented or *modeled* (*The data needs to be in **THIS** format, so I can understand it, i.e. my tools can easily parse and work with the data*).

# Modeling a LLDP Neighbor

Let's look at a real world example comparing Cisco Nexus and Arista devices since the APIs (*TRANSPORT*) are pretty similar and they are both using JSON.

If you issue the command `show lldp neighbors` over each device's API, you get structured data back in JSON.  Cisco supports XML too, but we are focused on JSON here for a like for like comparison with Arista.

Let's take a look.

Here is how a neighbor is modeled in JSON for a Nexus device using NX-API:

```
{
    "chassis_type": "7", 
    "chassis_id": "nx2", 
    "l_port_id": "Eth2/1", 
    "ttl": "120", 
    "capability": "1310740", 
    "port_type": "4149165831", 
    "port_id": "Eth2/1", 
    "mgmt_addr_type": "1953326957", 
    "mgmt_addr": "mgmt_addr"
}
```

And now here is how a neighbor is modeled in JSON for an Arista device using eAPI:

```
{
    "neighborDevice": "spine2.ntc.com", 
    "neighborPort": "Ethernet5", 
    "port": "Ethernet5", 
    "ttl": 120
}
```

You can easily tell the data is modeled differently and each vendor *defines* a neighbor how they think a neighbor should be defined.  Who's right?  Who's wrong? Neither...

If you were building an application to work the neighbors of both device types, you'd probably want to normalize the data --- to make the data easier to work with, manipulate, etc.

We can normalize data like this by using some mapping function between the vendor provided keys and user-defined keys that we want to use to model the data.  The mapping function could be raw code, a template, or whatever.  It doesn't matter.

After running the Arista neighbor through a sample mapping function, we get the following:

```
{
    "neighbor_interface": "Ethernet5", 
    "local_interface": "Ethernet5", 
    "neighbor": "spine2.ntc.com"
}
```

We now have the data in the format *we defined* using the keys `neighbor_interface`, `local_interface`, `neighbor`.  It's not too big of a pain to map keys and such as you can see, but imagine doing this for EVERY vendor, for lots of features --- even worse, what if the values were different data types and you wanted to normalize them too?  It'd be a mess (well, that's my daily life right now actually).

And after applying the same map to the Cisco neighbor:

```
{
    "neighbor_interface": "Eth2/1", 
    "local_interface": "Eth2/1", 
    "neighbor": "nx2"
}
```

We now have what we define a neighbor to be and can more easily write an application to consume this data.

Going one step further, this example doesn't validate the values are of a certain data type.  While, this is an easy one because they're all strings, often times vendors may return a list for one type of data and a dictionary for another, etc. making mapping functions much more complex to write!  This is where using something like [jsonschema](https://pypi.python.org/pypi/jsonschema) comes into play when working with JSON within Python.

Hopefully by now you can see that how data is modeled is actually more important than how the data is transported.  This is why its the focus of the OpenConfig working group.

Let's continue and now take a look at working with an OpenConfig data model in Python.

# OpenConfig Data Models

Every example thus far has been shown in native Python using dictionaries / JSON objects, but what OpenConfig is doing - is developing standardized data models using YANG.  We'll see soon how to work with a YANG model in Python.

You can check out the YANG data models for BGP [here](https://github.com/openconfig/public/tree/master/release/models/bgp).

If you looked at any of the examples, I bet you thought that was pretty fun to look at, right?  You must be thinking how does YANG help solve the data modeling problem and simplify working with multiple types of devices.  Don't worry, we'll get there soon enough.

Since YANG provides a standard way for writing data models, what would be really helpful is to have to tools to help us work with these data models.  Luckily, some great folks have created tools such as [pyang](https://github.com/mbj4668/pyang) and [pyangbind](https://github.com/robshakir/pyangbind), which are Python libraries to help with data validation and the creation of Python objects that adhere to actual YANG data models.

> Since _pyangbind_ builds on _pyang_, we are going to look at _pyangbind_.

As stated on the pyangbind GitHub [site](https://github.com/robshakir/pyangbind):

> PyangBind is a plugin for Pyang which converts YANG data models into a Python class hierarchy, such that Python can be used to manipulate data which conforms with a YANG data model.

What this is telling us is that we can now work in native Python and know for sure that we are building a valid OpenConfig configuration that 100% adheres to the associated OpenConfig YANG data model.  Thus, Pyangbind auto-generates Python bindings (class objects) based off the provided YANG model.  How cool is that?

After all the build up, let's finally look at an example code block that uses Python objects that were auto-generated from pyangbind to see how this all comes together.

```python
>>> from oc_bgp import bgp 

>>> oc = bgp()

>>> oc.bgp.global_.config.as_ = 2856

>>> oc.bgp.global_.config.router_id = "10.152.0.4"

>>> oc.bgp.neighbors.neighbor.add("192.168.1.2")

>>> oc.bgp.neighbors.neighbor["192.168.1.2"].config.peer_as = 5400

>>> oc.bgp.neighbors.neighbor["192.168.1.2"].config.description = "a fictional transit session"  
>>> 
```

> Note: The Python module called `oc_bgp.py` is what was created from pyangbind.

The next example shows what happens when you try and configure a BGP peer type with an invalid value. 

```python
>>> oc.bgp.neighbors.neighbor["192.168.1.2"].config.peer_type = "An Invalid Value"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: peer_type must be INTERNAL or EXTERNAL
>>>
>>> oc.bgp.neighbors.neighbor["192.168.1.2"].config.peer_type = "EXTERNAL"  
>>>
```

Because the python bindings being used are based off the actual BGP YANG model, it **ensures** that all configurations adhere to the model.  Sweet, right?

And here is the snippet from the YANG model that defines what the valid options are for defining a peer type:

```
  typedef peer-type {
    type enumeration {
      enum INTERNAL {
        description "internal (iBGP) peer";
      }
      enum EXTERNAL {
        description "external (eBGP) peer";
      }
    }
    description
      "labels a peer or peer group as explicitly internal or
      external";
  }
```

As you can see the valid options are "INTERNAL" and "EXTERNAL" - so in Python, an error is automatically raised when you enter values that don't adhere to the model as originally defined in YANG (of course this is only when using pyangbind).

We can also view the full configuration built as a single object.

```python
>>> print json.dumps(oc.get(filter=True), indent=4)
{
    "bgp": {
        "neighbors": {
            "neighbor": {
                "192.168.1.2": {
                    "neighbor-address": "192.168.1.2", 
                    "config": {
                        "peer-type": "EXTERNAL", 
                        "peer-as": 5400, 
                        "description": "a fictional transit session"
                    }
                }
            }
        }, 
        "global": {
            "config": {
                "as": 2856, 
                "router-id": "10.152.0.4"
            }
        }
    }
}

```

We now have a BGP configuration built off of a vendor-neutral data model.  Assuming a network device supports the BGP OpenConfig data model, the output generated from pyangbind can be sent directly to the device.  And remember, this was all done without any vendor-specific libraries.

If you do watch more of the sessions from the [Data Field Day Round table 1 Event](http://techfieldday.com/event/dfdr1/), you'll see that Cisco announced support for the OpenConfig BGP models on the IOS-XR Platform, which is pretty damn cool.  So in theory, we should be able push this object directly to a Cisco IOS-XR router.  I say in theory because I haven't tested it _yet_ first hand.

> Note: Cisco supports both NETCONF and [gRPC](http://www.grpc.io/) for transport which means in order to send the BGP object as shown above, it would need to be sent as XML or [protocol buffers](https://developers.google.com/protocol-buffers/?hl=en) (default data format for gRPC).

# Preparing for the Future

You may be thinking that OpenConfig is still in its infancy and so few devices support the OpenConfig data models, so what value can it provide today and now?

Actually, using these tools and models can prove to be a great first step toward **_open network automation_**.  Make sure you model all of your configurations using the OpenConfig data models.  It'll provide consistency across all vendors and ensure you are not using any vendor-proprietary features (although there are extensions in the models for this).

The only missing component then becomes a translation layer that converts the JSON object (as an example) from above into the appropriate API requests, objects, and/or CLI commands based on the particular device.  Of course, this _translation layer_ is only temporary until the device natively supports the OpenConfig data models.

> Want to see examples of OpenConfig data models _translated_ into CLI commands for a variety of vendors?  Check out this other post by [Rob Shakir](http://rob.sh/post/213).  And by the way, Rob is the one who wrote pyangbind, so he knows what he's talking about! 

> Best yet, Rob works for Jive Communications (a company that is not a multi-billion dollar company)- the point being that you do not have to work for _Google_ _or equivalent_ to get involved with OpenConfig.  Rob and Jive are paving the way to show what's possible when it comes to **open network automation** no matter the shape or size of your network.

# YANG != NETCONF

It's worth pointing out that while YANG is often tied to NETCONF (transport), it shouldn't be.  OpenConfig is showing that it's more than possible to de-couple YANG from NETCONF.  The OpenConfig working group is leveraging YANG to model not only configuration data, but also telemetry information coming from the device, and doing their best to get vendor support of these models.

# Summary

OpenConfig is made up of network operators such as Google, ATT, BT, Comcast, Facebook, Apple, Yahoo, Jive Communications, and many more.  The key is they are *users* driving the direction for the data models that make sense to implement for common workflows and design patterns.  It's **NOT** about developing a standard model for every feature the box supports.

Earlier in the post I made the statement that we would not see a common multi-vendor network device API because '*what's in it for the vendors?*'  If you can read between the lines, the value for vendors **is getting/obtaining/maintaining the business from the *LARGEST* operators on the planet**.  So, there is a huge cash opportunity for the large network vendors if they play nice with large operators and huge risk if they don't.

And what's important to those size operators now will become more important to *us normal folks* over time, so keep your eyes peeled! :)

In general, I'm super excited to be following the OpenConfig working group. This *may* be just the motivation vendors need to start building toward common data models, and who knows, maybe even a common transport type.

At some point, I do hope I can get more involved in projects like this (currently only operators can take part in the OC WG).  They are pretty public about their work; as an example you can see their current models on the [OpenConfig GitHub page](https://github.com/openconfig/public).

What do you think?  Will OpenConfig get wheels?  Will we see more models being supported by the vendors out there?


Thanks,
Jason

@jedelman8

