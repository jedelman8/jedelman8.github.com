---
layout: post
title: Network Test Automation with Ansible
tags: ansible, network automation
---

In the last [post](/home/ansible-for-networking), I talked about how Ansible could be used for various forms of network automation.  In the comments, Michael asked if Ansible could also be used for network test automation and verification.  Since I’m just starting to explore Ansible, I figured why not try it out.  The short answer is, it’s possible.  Let’s take a look at an example proving this out.

I just built a playbook that verifies a router, or group of routers, has reachability to a defined list of destinations.  Maybe you are deploying a new site? Maybe a new host?  Is the route being sent correctly to all sites?  Now, a playbook like this can be used to ensure every site (from the router) has reachability to the new site, subnet, or host.  You define the destination.  Cool, right?

Update: Before reading anymore, it's recommended to read the first post on [Ansible for Networking](/home/ansible-for-networking).

Let’s do a quick walk through.

Here is the playbook.

![playbook](/img/nta1.png)

As you can see, there is not a lot to it.  This playbook that we are looking at, called ‘pinger’, is using a module called ‘auto_pinger’.  Two variables get sent to auto_pinger: the first is the source_host, which is just the router that will be issuing the pings.  If you recall from the previous post, ‘inventory_hostname’ is just the name for the host located in the ‘hosts’ file.  

Now, how many devices and which devices, do you want to test reachability to?  remote_hosts is the directory where a file is located that has a list of these devices.  It’s that simple.

The task in the play in the playbook just states that each host in the group ‘routers’ is going to test connectivity to each host/destination defined in the file ‘remote_devices’ located in the ‘remote_hosts’ directory.  Note: file name must be called "remote_devices" for the destinations.  The path can be changed as a variable.

Let’s run the playbook.

![playbook-run](/img/nta2.png)

The test is run.  What just happened? How do we view the results?  Did the pings pass or fail?  Well, the good news is that the tasks were “OK.” This means for us that the playbook succeeded (pings were sent from the devices), but again, what are the results?

To document the results, log files for each device are created.  I actually create two log files per device.  One  detailed log file per device that captures more information than the normal user would want to see and then a simple log file.  I’m only going to review the simple log file. 

Here are the contents of log_simple_router1 after running the playbook:

![log](/img/nta3.png)

Note: [cpal](http://www.jedelman.com/1/post/2014/03/demo-common-programmable-abstraction-layer.html) is being used on the backend for connecting to the network devices.

*And by the way, it took this playbook just under 20 seconds to run.  If I wanted, I can probably get that down by a few seconds too.  How long does it take today if you want to test reachability to 4 destinations from 3 routers and write down the results?  Multiple that by 10, 20, or 100!*

We can easily see the results in the simple log file for each ping test.  Since I hadn’t shown the remote devices file yet, you can see that I have four devices being tested for reachability.  I am testing reachability between all of the routers and then also to google showing there is internet access.  And for those who would rather see the output in excel or a different format, that’s possible too.  Just means some changes to the module.

Running this playbook also produced log_simple_router2 and log_simple_router3.  Those logs files aren’t shown here.

As an alternative and also helpful for debugging, it is possible to run a playbook with a ‘-v’ option to see the data being returned.  Here is the output of that:

![pb-run](/img/nta4.png)

The results being returned are in a dictionary and within the dictionary are various variables and other data types.  Again, this data can be used to troubleshoot or it can be gathered to use in other tasks further down in the playbook.  So, let’s say all 5 pings fail.  Maybe after that, you want to re-run the ping test again to prove the remote device is definitely unreachable.  After proving it is unreachable, it would then be possible to check the routing table of the router, do a traceroute, etc.  Liking this so far?

The user of the playbook, likely a network engineer, would only have to modify the ‘hosts’ file and ‘remote_devices’ file.  That is it.  No hardcore programming at all! 

It wouldn't be complete without showing the hosts and remote_device files.

hosts file:

![hosts](/img/nta5.png)

remote_devices file:

![remote-hosts](/img/nta6.png)

The area of dynamic and automating testing is an interesting area and it may be one of those areas like automating the building of config files, where the proper tools and processes can be tested before starting to automate production devices.  

When you reboot a core switch, how many pings do you do to ensure everything is back up?  Stop that now!

With enough network centric Ansible modules, Ansible just may be the tool to help us network folk forward.

What do you think?  Would love to hear your thoughts.

Thanks,
Jason

PS – The playbook, files, and more are posted [online](https://github.com/jedelman8/ansible-auto-ping).  Feel to take a look.







