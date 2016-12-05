---
layout: post
tags: cisco, nexus, automation, ansible
title: Automating Cisco Nexus Switches with Ansible
---

For the past several years, the open source [network] community has been rallying around Ansible as a platform for network automation.  Just over a year ago, Ansible recognized the importance of embracing the network community and since then, has made significant additions to offer network automation _out of the box_.  In this post, we'll look at two distinct models you can use when automating network devices with Ansible, specifically focusing on Cisco Nexus switches.  I'll refer to these models as CLI-Driven and Abstraction-Driven Automation.  

> Note: We'll see in later posts how we can use these models and a third model to accomplish intent-driven automation as well.

For this post, we've chosen to highlight Nexus as there are more Nexus Ansible modules than any other network operating system as of Ansible 2.2 making it extremely easy to highlight these two models.

# CLI-Driven Automation

The first way to manage network devices with Ansible is to use the Ansible modules that are supported by a diverse number of operating systems including NX-OS, EOS, Junos, IOS, IOS-XR, and many more.  These modules can be considered _the lowest common denominator_ as they work the same way across operating systems requiring you to define the commands that you want to send to network devices.

We'll look at an example of this model managing VLANs on Nexus switches.

The first thing we are going to do is define an appropriate data model for VLANs.  The more complex the feature (and if you want to consider multiple vendors), the more complex and advanced a data model can be.  In this case, we'll simply create a list of key-value pairs and represent each VLAN by having a VLAN ID and VLAN NAME.

The data model we are going to use for VLANs is the following:

```yaml
---

vlans:
  - id: 10
    name: web_servers
  - id: 20
    name: app_servers
  - id: 30
    name: db_servers
```

Once we have our data (variables), we'll use a Jinja2 template that'll create the required configurations that we'll ultimately deploy to each device. 

Below is the Jinaj2 template we'll use to create our configurations.  In our template, we are requiring a VLAN name be present for each VLAN.

{% raw %}
```
{% for vlan in vlans %}
vlan {{ vlan.id }}
  name {{ vlan.name }}
{% endfor %}
```
{% endraw %}

Once the configuration commands are built from the template, we need to deploy (ensure the required commands exist) them to the device.  This is where we can use the **nxos_config** Ansible module.

> The *_config modules exist for a large number of network operating systems, which is why we're calling them the lowest common denominator.

We can create the required configurations and do the deployment with two Ansible tasks:

```yaml
    - name: BUILD CONFIGS
      template:
        src: vlans.j2
        dest: configs/vlans.cfg

    - name: ENSURE VLANS EXIST
      nxos_config:
        src: configs/vlans.cfg
        provider: {% raw %}"{{ nxos_provider }}"{% endraw %}
```

> You could use the `commands` parameter in the **nxos_config** task rather than using a separate template task; however, it's a little cleaner using templates.  Yes, this is subjective.

> You could also eliminate the task that uses the **template** module all together and just do `source: vlans.j2`, but adding the secondary step offers the flexibility of validating the build step (command generation) before a deployment.

> In this example, we are going to deploy the same VLANs to all devices.

If we run a playbook that has these tasks, it'll ensure all VLANs in the variables file get deployed on the Nexus switches. 

But, what if you need to subsequently remove VLANs?  

In this current model, it's up to you to either create a second template that uses `no vlan <id>` or make a more complex template and based on some other variable, render the commands to either configure or un-configure VLANs.

# Abstraction-Driven Automation

Using the **nxos_config** module is simply one way you can manage NX-OS devices with Ansible.  While it offers flexibility, you still need to develop templates or commands for everything you want to configure or un-configure on a given device.  For some tasks, this is quite easy; for others, it could get tedious.  

Another approach is to use Ansible modules that offer abstractions, eliminate the need to use commands or templates, while also making it quite easy to ensure a given configuration does NOT exist.

We aren't talking about high level abstractions here such as services or tenants, but still network-centric objects and abstractions such as VLANs without the need for you to define the commands that are needed for configuring a given resource or object.

> You can in fact stitch together multiple network-centric objects to create a higher level abstraction such as tenants quite easily with Ansible.

Let's take a look at the same VLANs example from above, but instead of using **nxos_config**, we are going to use **nxos_vlan**.  

This module only manages VLANs globally on Nexus switches.

Using this model, the single task we need to ensure the VLANs exist on each switch is the following:

```yaml
    - name: ENSURE VLANS EXIST
      nxos_vlan:
        vlan_id: {% raw %}"{{ item.id }}"{% endraw %}
        name: {% raw %}"{{ item.name }}"{% endraw %}
        state: present
        provider: {% raw %}"{{ nxos_provider }}"{% endraw %}
      with_items: {% raw %}"{{ vlans }}"{% endraw %}
```

And if we need to remove the same VLANs:

```yaml
    - name: ENSURE VLANS DO NOT EXIST
      nxos_vlan:
        vlan_id: {% raw %}"{{ item.id }}"{% endraw %}
        name: {% raw %}"{{ item.name }}"{% endraw %}
        state: absent
        provider: {% raw %}"{{ nxos_provider }}"{% endraw %}
      with_items: {% raw %}"{{ vlans }}"{% endraw %}
```

The only change required to ensure the VLAN does not exist is to change the `state` parameter to **absent**.  

If you take a look at the [Ansible docs for network modules](http://docs.ansible.com/ansible/list_of_network_modules.html#nxos) you can see how many Nexus modules exist now in Ansible core.  For those new to Ansible, being in Ansible _Core_ means you get them when you install Ansible. 

Here is a list of all the Nexus modules that you'll also find at the Ansible docs link above.

```
nxos_aaa_server_host.py         nxos_hsrp.py            nxos_pim.py             nxos_udld_interface.py
nxos_aaa_server.py              nxos_igmp_interface.py  nxos_pim_rp_address.py  nxos_udld.py
nxos_acl_interface.py           nxos_igmp.py            nxos_ping.py            nxos_vlan.py
nxos_acl.py                     nxos_igmp_snooping.py   nxos_portchannel.py     nxos_vpc_interface.py
nxos_bgp_af.py                  nxos_install_os.py      nxos_reboot.py          nxos_vpc.py
nxos_bgp_neighbor_af.py         nxos_interface_ospf.py  nxos_rollback.py        nxos_vrf_af.py
nxos_bgp_neighbor.py            nxos_interface.py       nxos_smu.py             nxos_vrf_interface.py
nxos_bgp.py                     nxos_ip_interface.py    nxos_snapshot.py        nxos_vrf.py
nxos_command.py                 nxos_mtu.py             nxos_snmp_community.py  nxos_vrrp.py
nxos_config.py                  nxos_ntp_auth.py        nxos_snmp_contact.py    nxos_vtp_domain.py
nxos_evpn_global.py             nxos_ntp_options.py     nxos_snmp_host.py       nxos_vtp_password.py
nxos_evpn_vni.py                nxos_ntp.py             nxos_snmp_location.py   nxos_vtp_version.py
nxos_facts.py                   nxos_nxapi.py           nxos_snmp_traps.py      nxos_vxlan_vtep.py
nxos_feature.py                 nxos_ospf.py            nxos_snmp_user.py       nxos_vxlan_vtep_vni.py
nxos_file_copy.py               nxos_ospf_vrf.py        nxos_static_route.py    nxos_gir_profile_management.py
nxos_overlay_global.py          nxos_switchport.py      nxos_gir.py             nxos_pim_interface.py

```

## Beyond Configuration Management

Notice that not only are there a significant amount of modules for configuration management, but there are also quite a few for common operational tasks such as copying files to devices, rebooting devices, testing reachability (using ping), upgrading devices, and rolling back configurations (using the NX-OS checkpoint feature).

Let's look at one of these.  We'll walk through how you can use multiple tasks in a given playbook to upgrade the operating system on Nexus switches.  

## Upgrading NX-OS Devices

While there is one module called **nxos_install_os** and that module does perform the actual upgrade, we'll walk through the complete process starting with checking to see the current version of software on the Nexus switches and finishing with a task to assert the upgrade completed successfully.

The first three tasks we are going to have in our playbook are the following:

1. Get current facts of each device so we can print the current version of NX-OS to the terminal during playbook execution
2. Print (debug) the current version of NX-OS to the terminal
3. Ensure the SCP server is enabled on each device so we can copy files 

Here is what these tasks look like:

```yaml
- name: GATHER FACTS TO RECORD CURRENT VERSION OF NX-OS
  nxos_facts:
    provider: {% raw %}"{{ nxos_provider }}"{% endraw %}

- name: CURRENT OS VERSION
  debug: var=os

- name: ENSURE SCP SERVER IS ENABLED
  nxos_feature:
    feature: scp-server
    state: enabled
    provider: {% raw %}"{{ nxos_provider }}"{% endraw %}
```

> Note: `nxos_provider` is a variable that contains common parameters for the nxos_* modules.  In this particular case, it's a dictionary that has the following keys:  `username`, `password`, `transport`, and `host`.

After we know that **scp** is enabled, we can confidently copy the required file(s) to each device.  Our example requires the OS image file exists on the Ansible control host.

```yaml
- name: ENSURE FILE EXISTS ON DEVICE
  nxos_file_copy:
    local_file: {% raw %}"{{ image_path }}/{{ nxos_version }}"{% endraw %}
    provider: {% raw %}"{{ nxos_provider }}"{% endraw %}
```

In addition to `nxos_provider`, we used two other variables to define the `local_file` parameter.  They are defined as the following:

```yaml
nxos_version: nxos.7.0.3.I2.2d.bin
image_path: ../os-images/cisco/nxos
nxos_version_str: 7.0(3)I2(2d)
```

But, we also have a third variable that is the string representation of the new version (`nxos_version_str`), which we'll use in an upcoming task. 

> Based on your deployment, think about using the **delegate_to** directive when using **nxos_file_copy** so you can copy files from another host in the data center, or some location that is closer to your devices than the Ansible control host.

Once we copy the image file to each switch, we're ready to perform the final step: the upgrade.  It sounds like a single task, but in reality, it's a bit more than that.  We'll summarize these steps as the following:

1. Start the Upgrade (which initiates the reboot) 
2. Ensure the device starts rebooting
3. Ensure the device comes back online
4. Gather facts again to collect new OS version 
5. Print (debug) the OS to the terminal
6. Assert and Verify the expected version is running on the device

If we translate these steps into Ansible tasks, we end up with the following:

```yaml

- block:
    - name: ENSURE OS IS CORRECT
      nxos_install_os:
        system_image_file: {% raw %}"{{ nxos_version }}"{% endraw %}
        provider: {% raw %}"{{ nxos_provider }}"{% endraw %}
  rescue:
    - name: WAITING FOR DEVICE TO PERFORM ALL UPGRADE CHECKS AND STARTS REBOOT
      wait_for:
        port: 22
        state: stopped
        timeout: 300
        delay: 60
        host: {% raw %}"{{ inventory_hostname }}"{% endraw %}

    - name: REBOOT IN PROGRESS - WAITING FOR DEVICE TO COME BACK ONLINE
      wait_for:
        port: 22
        state: started
        timeout: 300
        delay: 60
        host: {% raw %}"{{ inventory_hostname }}"{% endraw %}

  always:

    - name: GATHER FACTS TO RECORD CURRENT VERSION OF NX-OS
      nxos_facts:
        provider: {% raw %}"{{ nxos_provider }}"{% endraw %}

    - name: CURRENT OS VERSION
      debug: var=os

    - name: VERIFY CURRENT VERSION IS EXPECTED VERSION
      assert:
        that:
          - {% raw %}"'{{ os }}' == '{{ nxos_version_str }}'"{% endraw %}
```

As you read through the preceding tasks, take note of the feature being used within the playbook called Ansible blocks.  This is a critical feature to be aware of to account for errors when running a playbook and conditionally execute a group of tasks when an error occurs.  Because Ansible doesn't yet allow for a device to lose connectivity within a task, we need to _assume_ a failure is going to occur.  Basically, whenever you use **nxos_install_os**, the task **will** fail when the switch reboots (for now).  Within a block, when a failure condition occurs, the tasks within the _rescue_ block start executing.  

The **rescue** tasks shown above ensure the device starts rebooting within 5 minutes and ensure the device comes back online within 5 minutes.  Note that these tests were executed against Nexus 9396s and 9372s - if you are using 7K or 9K chassis devices, you may want to increase the timeout values.

Finally, the last three tasks _always_  get executed and verify the upgrade was successful.

In upcoming posts, we'll take a look at a few other use cases and show some live demos.  Over time, we'll get more complete examples on GitHub too.



Thanks,

Jason

@jedelman8




