---
layout: post
tags: juniper, automation, ansible
title: Juniper vSRX Automation with Ansible
---

Virtual appliances not only provide for a great lab environment, but are the future of how network services will be tested, validated, and delivered within an Enterprise.  And Juniper gets this – they spent a lot of time covering the [vSRX](http://vimeo.com/136977916) and [vMX](http://vimeo.com/136979030) product lines at the most recent [Networking Field Day event](http://techfieldday.com/event/nfd10/).

Over the next few months, I’ll more than likely be spending a lot of time on Juniper gear, and it will be the virtual platforms, so it was good timing to get to be in the room to learn more about them along with many of the automation capabilities Juniper supports across their product families.

# NETCONF Rules All for Juniper

While I have not spent as much time on Juniper kit as I would have liked over the past few years, the one awesome thing to see and experience first-hand is that they have a unified API (NETCONF) across all of their products.

Why is this so valuable?  Well, for one, we get to use the same libraries and integrations across platforms.  As an example, we can use the Juniper Ansible modules across any of their devices.  In this post, we’ll take a look at using one of them to bootstrap a vSRX. 

I started with a default configuration out of the _box_ and just booted up the vSRX on a vSphere host.

These commands were added in the meantime to enable management connectivity and get a hostname configured:

# Loading Base Commands

```
set system host-name vsrx
set system root-authentication plain-text-password
set interfaces fxp0 unit 0 family inet address 10.0.0.15/24
set routing-options static route 0.0.0.0/0 next-hop 10.0.0.2
set system services netconf ssh 
```

That resulted in this configuration:

```
system {
    host-name vsrx-1;
    root-authentication {
        encrypted-password "$1$M6nEzy6d$lnFk35FdC2xvf9p7v0JHT."; ## SECRET-DATA
    }
    services {
        ssh;
        netconf {
            ssh;
        }
        web-management {
            http {
                interface fxp0.0;
            }
        }
    }
    syslog {
        user * {
            any emergency;
        }
        file messages {
            any any;
            authorization info;
        }
        file interactive-commands {
            interactive-commands any;
        }
    }
    license {
        autoupdate {                    
            url https://ae1.juniper.net/junos/key_retrieval;
        }
    }
}
security {
    screen {
        ids-option untrust-screen {
            icmp {
                ping-death;
            }
            ip {
                source-route-option;
                tear-drop;
            }
            tcp {
                syn-flood {
                    alarm-threshold 1024;
                    attack-threshold 200;
                    source-threshold 1024;
                    destination-threshold 2048;
                    timeout 20;
                }
                land;
            }
        }
    }
    policies {
        from-zone trust to-zone trust {
            policy default-permit {
                match {
                    source-address any; 
                    destination-address any;
                    application any;
                }
                then {
                    permit;
                }
            }
        }
        from-zone trust to-zone untrust {
            policy default-permit {
                match {
                    source-address any;
                    destination-address any;
                    application any;
                }
                then {
                    permit;
                }
            }
        }
        from-zone untrust to-zone trust {
            policy default-deny {
                match {
                    source-address any;
                    destination-address any;
                    application any;
                }
                then {
                    deny;
                }
            }                           
        }
    }
    zones {
        security-zone trust {
            tcp-rst;
        }
        security-zone untrust {
            screen untrust-screen;
        }
    }
}
interfaces {
    fxp0 {
        unit 0 {
            family inet {
                address 10.0.0.15/24;
            }
        }
    }
}
routing-options {
    static {
        route 0.0.0.0/0 next-hop 10.0.0.2;
    }
}


```

# Ansible Prep

Now, let's use Ansible to apply a full [NEW] configuration in real-time to be the new running configuration.  Specifically, we'll be using the `junos_install_config` module.

## Install JunOS Python Library
First install the required Juniper library:

```
sudo pip install junos-eznc --upgrade
```

## Install Juniper Ansible Modules

Now install the Juniper Ansible modules:

```
git clone https://github.com/Juniper/ansible-junos-stdlib.git
```

Navigate to the `ansible-junos-stdlib` directory that was just created when you cloned the repo.

```
cd ansible-junos-stdlib
```

## Save new Config

The new configuration file is stored as `new_firewall.conf` and should exist within the `ansible-junos-stdlib` directory.  For testing purpose, I've only changed the hostname from the config shown above to be `VSRX-NTC`.

## Create Ansible Playbook

I'll create a new Ansible playbook within the `ansible-junos-stdlib` directory that looks like this:

{% raw %}
```
---

- name: demo vsrx
  hosts: vsrx.networktocode.com
  gather_facts: no
  connection: local

  tasks:

    - name: INSTALL JUNOS CONFIG
      junos_install_config:
        host={{ ansible_ssh_host }}
        file=new_firewall.conf
        user=root
        passwd=xxxxx
        logfile=deploysite.log
        overwrite=yes
        diffs_file=deploysitediffs.logs
```
{% endraw %}


The playbook was saved as `juniper-srx.yml` (in the same directory as the config file).

## Create Ansible Inventory File

The inventory file called `hosts`, also in the same directory, looks like this:

```
vsrx.networktocode.com  ansible_ssh_host=153.92.34.36

```

Note: I am NATing the IP shown in the inventory file to the private mgmt IP shown in the config file.

## Running the Playbook

If we run the playbook, we'll see the following output on the terminal:

```
cisco@ntc:~/ansible-junos-stdlib$ ansible-playbook -i hosts juniper-srx.yml -v

PLAY [demo vsrx] ************************************************************** 

TASK: [INSTALL JUNOS CONFIG] ************************************************** 
changed: [vsrx.networktocode.com] => {"changed": true, "file": "/home/cisco/ansible-junos-stdlib/new_firewall.conf"}

PLAY RECAP ******************************************************************** 
vsrx.networktocode.com     : ok=1    changed=1    unreachable=0    failed=0   

cisco@ntc:~/ansible/testing/ansible-junos-stdlib$ 

```

Going back to an SSH session to the vSRX, we can see that the hostname has successfully changed!!!

```
root@VSRX-NTC# 

[edit]
root@VSRX-NTC# 
```

And just like that within a few minutes you could be automating Juniper devices too!

Thanks,
Jason

@jedelman8



