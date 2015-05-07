---
layout: post
title: Server Bootstrap with Ansible
tags: ansible
---

Over the past few months, I’ve been posting on using Ansible for network automation.  Changing things up a bit, this post will cover using Ansible for server automation and I’ll share a few Ansible playbooks that I’ve built and have been using to bootstrap servers and prep them for various applications such as OpenStack and NSX deployments.  

### Step 1 - Playbook 1
*Creating password-less root account*

Since Ansible uses SSH by default for connecting to the servers, you will realize the first thing that needs to be done is to copy the public key of where you will execute playbooks from onto the “new” server.  To do this, I use a playbook that is called server_one_time_run.yml.  You will notice that in this playbook, and only in this playbook, I have remote_user set to jedelman and sudo set to yes.

I’ve been testing against bare-metal and virtual machine installs using an Ubuntu ISO image.  During the OS install process, “jedelman” is the account that was created on all hosts and virtual machines. 

This playbook runs and copies over the public key in the root directory.  We are essentially creating a password-less login for the root account, so going forward, all other playbooks can be run using the root account.  Realize this may not be best practice!

Three other things to remember and take note of:

* This first playbook needs to be with the --ask-sudo-pass and –k options.  This allows you to enter the SSH and sudo passwords for the account you created upon installing your Linux OS while running the playbook.  I was debugging ssh for longer than I’d like to admit to figure out the –k was needed
* You also need to install openssh-server (or another of your choosing) on the server before running this playbook.
* If like me and you are doing many re-builds in a short amount time, you may need to update/remote hosts from the local known_hosts file.  The ssh-keygen command can be used here, if needed.

After running this playbook, you can ssh to one of your servers with the root user and ensure you aren’t being prompted for a password.

### Step 2 - Playbook 2
*Baseline Apps Install*
 
The second playbook that I’ve been running gets a slew of apps installed onto the system, but first adds the Ubuntu Cloud Archive (for OpenStack Icehouse) to host’s source list.  This playbook’s name is base_apps.yml.  Note that all playbooks going forward are using “remote_user: root” 

I was hitting a bug using older versions libvirt and OVS (from the standard repo) --- using the cloud archive cleared it up right away.  So net net, don’t install OVS or libvirt until after the Ubuntu Cloud key ring is added and an update and upgrade are performed, especially if you want to test OVS and libvirt with VLANs.  You can actually find this in some of the OpenStack install documentation as well.

This playbook finishes by adding a new user account and installing xrdp.  I’m just toying with building some hosted labs and “netops” is the account that I was using to login to rdp although any of them will work.

### Step 3 - Playbook 3
*OVS Install*
 
The next step is to get OVS installed.  This is using the ovs-default.yml playbook.  Because I was testing with NSX, I used a few OVS files specific to NSX.  This playbook copies the NSX files from my machine running Ansible to each hypervisor node and installs each .deb after the initial package was untar’d.  Assuming most of you aren't testing this with NSX, you can use the off the shelf OVS files.

### Step 4 - Playbook 4
*OVS & Network Config*
 
Once OVS is installed, the next step will be to get it configured. This time I used the playbook called ovs-config.  Because I was testing a few different variations not always using a management interface that wasn’t running OVS, many of the lines are still commented out, but if you had a dedicated management interface, most of it should run right through.  You can still try it, but you’ll get disconnected after making a change to the NIC you are SSH’d in through :). 

The physical servers I’m testing with have 2 physical NICs.  Most often, one was being used for management and one was used for production, or virtual machine, traffic. A single OVS bridge was being used for each and they are carrying multiple VLANs over the links.  Note: I only have br-mgmt’s config in the playbook right now.

Note: there is more background that is required to understand libvirt with OVS and VLANs.  This is not that post although you should be able to learn more about it by reading through the playbooks.

You will also notice in line 31 in ovs-config.yml, there is a template file being copied over (again, from my machine) onto the target node.  This is replacing the /etc/network/interfaces file on the target machine with the updated settings I wanted for each node.

While I was going through the libvirt, VLANs, and OVS build out, Scott Lowe (@scott_lowe) was a huge help.  Big shout out to Scott!!  Hopefully, in the near future, I can write a single post on that without Ansible to articulate some nuances encountered along the way!

### Files

All playbook information described can be found on my github page [here](https://github.com/jedelman8/server-build-ansible).


Thanks,
Jason

Twitter: @jedelman8
