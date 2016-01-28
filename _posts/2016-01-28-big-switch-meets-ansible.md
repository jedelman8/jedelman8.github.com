---
layout: post
tags: big switch, ansible, automation
title: Big Switch Meets Ansible
---

Big Switch offers [on demand labs](labs.bigswitch.com) to get instant access to Big Cloud Fabric (BCF) and Big Monitoring Fabric (BMF).  Using these labs, it's quite easy to experience the products first hand and see what they are all about.  The labs also come with lab guides that walk you through step-by-step on how to get started using BMF and BCF.

For me, one of the more appealing aspects of these labs is that Big Switch also exposes the APIs such that you can access them directly from your personal machine.  This makes it possible to not only test the product, but also test the API on each controller platform (BMF and BCF).

The best part is, you don't even need to use any docs because they offer a command that shows the API calls being made by certain show commands.  

```
controller> debug rest
***** Enabled display rest mode *****
REST-SIMPLE: GET http://127.0.0.1:8080/api/v1/data/controller/core/controller/role
controller> 
```

Like the output from a `show version`? Ensure `debug rest` is enabled, and then just issue the command to grab the APIs being called to generate the text output on the CLI.

```
controller> show version
REST-SIMPLE: GET http://127.0.0.1:8080/api/v1/data/controller/core/version/appliance
REST-SIMPLE: http://127.0.0.1:8080/api/v1/data/controller/core/version/appliance done, 0:00:00.012266
~~~~~~~~~~~~~~~~~~~~~~~~~~~ Appliance  ~~~~~~~~~~~~~~~~~~~~~~~~~~~
Name            : Big Cloud Fabric Appliance
Build date      : 2015-12-20 07:06:05 UTC
Build user      : bsn
Ci build number : 75
Ci job name     : bcf-3.5.0
Release string  : Big Cloud Fabric Appliance 3.5.0 (bcf-3.5.0 #75)
Version         : 3.5.0
REST-SIMPLE: GET http://127.0.0.1:8080/api/v1/data/controller/core/controller/role
REST-SIMPLE: http://127.0.0.1:8080/api/v1/data/controller/core/controller/role done, 0:00:00.013763
```

We can see that one of the APIs being called is this:

```
GET http://127.0.0.1:8080/api/v1/data/controller/core/version/appliance
```

Pretty cool.

This has to be going somewhere, right?  Absolutely...can't just *play* with an API and not build anything.  

# Big Switch Ansible Modules

Over the past few days, I spent time working with these APIs and ended up developing Ansible modules to collect facts for BMF and BCF.  They can be found [here](https://github.com/networktocode/bsn-ansible).

Facts is a great place to get started because you can now use this information to use as inputs into other modules (more in the future!) and even more important, the facts collected can be used to create dynamic reports, etc.  

So, if you're using BMF and/or BCF, these modules can be used to help keep track of that inventory or to do health checks in real-time.

Here is a sample playbook using the module **bcf_get_facts** to collect facts from Big Cloud Fabric.

```yaml
---

   - name: GATHER FACTS FROM BIG SWITCH CONTROLLERS
     hosts: bcf
     connection: local

     tasks:

       - name: GATHER FACTS
         bcf_get_facts: controller={{ controller_ip }} username={{ un }} password={{ pwd }}

       - name: DUMP FACTS TO TERMINAL
         debug: var=bsnbcf
```

The facts returned is a dictionary and are stored in the key `bsnbcf` or `bsnbmf` based on which module you are using.

This is an example of facts gathered from Big Cloud Fabric:

```
{
    "var": {
        "bsnbcf": {
            "cluster": {
                "controllers": [
                    {
                        "hostname": "10.10.12.20",
                        "role": "active",
                        "uptime": 33807223
                    }
                ],
                "description": null,
                "name": "bigswitchcluster",
                "redundancy_status": {
                    "msg": "Single node configured",
                    "status": "standalone"
                },
                "virtual_ip": null
            },
            "fabric_nodes": [
                {
                    "dpid": "00:00:00:00:00:02:00:01",
                    "fabric_state": "connected",
                    "name": "R1L1",
                    "role": "leaf",
                    "sw": "Switch Light Virtual 3.5.0 2015-12-15.00:38-db7144c trusty-amd64"
                },
                {
                    "dpid": "00:00:00:00:00:02:00:02",
                    "fabric_state": "connected",
                    "name": "R1L2",
                    "role": "leaf",
                    "sw": "Switch Light Virtual 3.5.0 2015-12-15.00:38-db7144c trusty-amd64"
                },
                {
                    "dpid": "00:00:00:00:00:02:00:03",
                    "fabric_state": "connected",
                    "name": "R2L1",
                    "role": "leaf",
                    "sw": "Switch Light Virtual 3.5.0 2015-12-15.00:38-db7144c trusty-amd64"
                },
                {
                    "dpid": "00:00:00:00:00:02:00:04",
                    "fabric_state": "connected",
                    "name": "R2L2",
                    "role": "leaf",
                    "sw": "Switch Light Virtual 3.5.0 2015-12-15.00:38-db7144c trusty-amd64"
                },
                {
                    "dpid": "00:00:00:00:00:02:00:05",
                    "fabric_state": "connected",
                    "name": "R3L1",
                    "role": "leaf",
                    "sw": "Switch Light Virtual 3.5.0 2015-12-15.00:38-db7144c trusty-amd64"
                },
                {
                    "dpid": "00:00:00:00:00:02:00:06",
                    "fabric_state": "connected",
                    "name": "R3L2",
                    "role": "leaf",
                    "sw": "Switch Light Virtual 3.5.0 2015-12-15.00:38-db7144c trusty-amd64"
                },
                {
                    "dpid": "00:00:00:00:00:01:00:01",
                    "fabric_state": "connected",
                    "name": "S1",
                    "role": "spine",
                    "sw": "Switch Light Virtual 3.5.0 2015-12-15.00:38-db7144c trusty-amd64"
                },
                {
                    "dpid": "00:00:00:00:00:01:00:02",
                    "fabric_state": "connected",
                    "name": "S2",
                    "role": "spine",
                    "sw": "Switch Light Virtual 3.5.0 2015-12-15.00:38-db7144c trusty-amd64"
                },
                {
                    "dpid": "00:00:00:00:00:01:00:03",
                    "fabric_state": "connected",
                    "name": "S3",
                    "role": "spine",
                    "sw": "Switch Light Virtual 3.5.0 2015-12-15.00:38-db7144c trusty-amd64"
                }
            ],
            "hostname": "controller",
            "platform": "Big Cloud Fabric Appliance 3.5.0 (bcf-3.5.0 #75)",
            "summary": {
                "controllers": 1,
                "errors": 18,
                "leaf_groups_configured": 3,
                "leaves_configured": 6,
                "leaves_connected": 6,
                "overall_status": "NOT OK",
                "spines_configured": 3,
                "spines_connected": 3,
                "tenants": 0,
                "vswitches_connected": 0,
                "warnings": 14
            },
            "vendor": "big_switch_networks",
            "version": "3.5.0"
        }
    }
}
```

And here is a sample from Big Monitoring Fabric:

```
    "var": {
        "bsnbmf": {
            "cluster": "N/A",
            "fabric_nodes": [
                {
                    "alias": "Filter-Switch",
                    "dpid": "00:00:00:00:00:00:00:0b",
                    "serial_number": "",
                    "sw": "Switch Light Virtual 3.1.0 2015-10-07.00:17-60a8572 trusty-amd64"
                },
                {
                    "alias": "Core-Switch",
                    "dpid": "00:00:00:00:00:00:00:0c",
                    "serial_number": "",
                    "sw": "Switch Light Virtual 3.1.0 2015-10-07.00:17-60a8572 trusty-amd64"
                },
                {
                    "alias": "Delivery-Switch",
                    "dpid": "00:00:00:00:00:00:00:0d",
                    "serial_number": "",
                    "sw": "Switch Light Virtual 3.1.0 2015-10-07.00:17-60a8572 trusty-amd64"
                }
            ],
            "hostname": "N/A",
            "platform": "Big Tap Controller 5.5.0 (2015.10.14.1909-m.bsc.bigdb)",
            "summary": {
                "active_policies": 0,
                "core_interfaces": 4,
                "delivery_interfaces": 2,
                "delivery_switches": 1,
                "filter_interfaces": 1,
                "filter_switches": 1,
                "match_mode": "bigtap-l3l4",
                "num_policies": 0,
                "num_services": 0,
                "service_interfaces": 2,
                "service_switches": 1,
                "total_switches": 3
            },
            "vendor": "big_switch_networks",
            "version": "N/A"
        }
    }
}
```


Feel free to contribute: [https://github.com/networktocode/bsn-ansible](https://github.com/networktocode/bsn-ansible)

Happy Automating.


Thanks,
Jason
@jedelman8






