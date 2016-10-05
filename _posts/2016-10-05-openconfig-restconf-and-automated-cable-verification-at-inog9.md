---
layout: post
tags: automation, ansible, openconfig
title: OpenConfig, RESTCONF, and Automated Cable Verification at iNOG9
---

Last week I was in Dublin for business which just so happened to overlap with [iNOG9](http://www.meetup.com/Irish-Network-Operators-Group/events/227495670/), which was last Wednesday.  As luck would have it, I had the opportunity to speak at iNOG9 about network automation.

You can watch the video if you want to see the [presentation](https://www.youtube.com/watch?v=fS_q7o98JKI), but the three mini demos I gave were: 

1. Cable verification on Juniper vMX devices using Ansible
2. Automating BGP on IOS-XR using OpenConfig BGP models using Ansible
3. Using Postman to explore and demo the new RESTCONF/YANG interface on IOS XE.

Few words about each.

## Cable verification

Usually when the topic of network automation comes up, configuration management is _assumed_.  It should **not** be as there are so many other forms and types of automation.  Here I showed how we can verify cabling (via neighbors) is accurate on a Junos vMX topology.  Of course, the hard part here is having the discipline to define the desired cabling topology first.  Note: links for sample playbooks can be found below on the GitHub repo.

## OpenConfig BGP Automation with Ansible

I built a custom Ansible module built around NETCONF (ncclient), but uses the OpenConfig YANG model for global BGP configuration.  For example, this is the XML representation of this YANG model that would be pushed over NETCONF:

> Note: this is why Ansible is a great and extensible platform as this was a custom module built in just a few hours for this demo/presentation.

```xml
<config>
 <bgp xmlns="http://openconfig.net/yang/bgp" nc:operation=create>
  <global>
   <config>
    <as>65512</as>
    <router-id>100.1.1.1</router-id>
   </config>
  </global>
 </bgp>
</config>
```

In theory, the Ansible task below should work on any vendor supporting NETCONF + OC-BGP.  Over time, we'll try and add gRPC and RESTCONF transport types too.  Yes, it's basic now as it only supports BGP AS and ROUTER ID, but the point is to show what vendor neutral data models offer.


```yaml
      - name: ENSURE DEVICES HAVE PROPER ASN AND RID
        oc_bgp:
          username: "{{ un }}"
          password: "{{ pwd }}"
          host: "{{ inventory_hostname }}"
          asn: 65536
          router_id: 10.1.1.1
          state: present
```

The **oc_bgp** Ansible module can found at the link below along with the sample playbook used at iNOG, etc.

_Thanks to @GabrieleGerbino for helping update some portions of the code._

## RESTCONF on IOS-XE

I only had 20 short minutes to speak at iNOG and wish I could have spent more time showing off the new RESTCONF (and NETCONF) interfaces on IOS XE  (yes, they are fully driven by YANG models).  The RESTCONF interface is pretty awesome if I may say - it's an http wrapper for using the same models that have been predominantly exposed by NETCONF/XML interfaces on other device types, but now with RESTCONF we can access them with native REST and use JSON or XML encoding!

Even better, and this is what I demo'd, is the proper implementation of HTTP verbs for a REST API.  The example I gave was this:

The following API call as a HTTP **PATCH** simply _adds_ two routes to the static routing table.

```
 http://csr/restconf/api/config/native/ip/route  (PATCH)
```

```
{
  "ned:route": {
    "ip-route-interface-forwarding-list": [
      {
        "prefix": "10.1.20.0",
        "mask": "255.255.255.0",
        "fwd-list": [
          {
            "fwd": "10.0.0.2"
          }
        ]
      },
      {
        "prefix": "10.1.30.0",
        "mask": "255.255.255.0",
        "fwd-list": [
          {
            "fwd": "10.0.0.2"
          }
        ]
      }
    ]
  }
}
```

This is standard and most commonly done today by network operators using the CLI.

Here is the magic though.  The next API call using a HTTP **PUT** declaratively states that this route(s) should be the only routes that exist in the static routing table - this means any other route in the static routing table will be purged.

```
 http://csr/restconf/api/config/native/ip/route  (PUT)
```

```
{
  "ned:route": {
    "ip-route-interface-forwarding-list": [
      {
        "prefix": "0.0.0.0",
        "mask": "0.0.0.0",
        "fwd-list": [
          {
            "fwd": "10.0.0.2"
          }
        ]
      }
    ]
  }
}

```


The end result is a single default static route in the static RT.

This is massively valuable. This means you do NOT need any command negations, i.e. imagine having a 100 static routes and you wanted to remove them all today.  How would you do it today?  How will you do it tomorrow? Command negations is one of the barriers to get over with any form of automation.

## Closing

As always, any questions, just reach out.  All materials from this presentation including slides, playbooks, and files can be  found [here](https://github.com/networktocode/inog9).

The video from iNOG9 can be found [here](https://www.youtube.com/watch?v=fS_q7o98JKI).

Thanks,

Jason

@jedelman8



