---
layout: post
title: Leveraging Python on Network Devices to Monitor Interfaces in Realtime 
tags: cisco, nexus, python
---

In a recent post, I wrote about some Python work I was testing on the Nexus 3000.  The end conclusion was that open Linux platforms will offer more flexibility --- for the consumer of the technology, ultimately the customer.  In this post, we’ll take a look at an example that integrates Python with the native Linux operating system.  

In the context of networking, the question often arises, what does having access to Linux really gain you?  For one, as you can see from my last post and you’ll see in this one, for native scripting within bash and Python is of extreme value in itself, not to mention you’d also have the ability to load any piece of software you want to that is compatible with Linux (think about tools, mgmt/monitoring platforms, etc.).

Okay, so you’d have the ability to use Python on a network switch.  So what?  What about running onboard analytics on the switch?  What about sending the exact data you need, the data you use to troubleshoot, the data part of your operational workflow, directly upstream to a head end server, or just simply to an existing syslog server.  Think about getting exactly what you want from each device rather than being told what you can have access to.

In this example application, the goal was 3-fold:  1) Build a lightweight network centric client/server application, but actually make the server optional 2) Learn and test basic Python network socket programming and 3) Run some basic analytics on the switch

The use case was detecting errors on an interface and being alerted if there were more than a certain amount of transmit errors in a given time interval.  As you’ll see in the code, I was detecting more than 3 packets sent in a 5 second interval, but the errors are there.  Using packets sent was easier to simulate and test.  It’s possible for SNMP to be polling for data like this, but usually the polling intervals are too long to actually capture the data desired.  Here, the agent on the Linux device that I call FBIAgent, will constantly be examining the packets/errors, etc. and will alert the head end server  (no polling required in this case).  As stated earlier the goal was to make the head end optional.  It’s actually optional because instead of triggering an alert through the socket built in Python, you can simply generate a normal syslog message if that is easier, and more importantly REACT on the event!

The high level architecture and flow goes like this:

* FBIAgent actively issues a ‘netstat –i’
* The result is then stored in a file.  This didn’t have to be stored in a file, but felt it was easier to parse individual versus parsing a string.
* The result from the file is then parsed and all interfaces starting with ‘eth’ are entered into individual dictionaries.  By the way, dictionaries are pure awesomeness in Python
* Actually, a dictionary of dictionaries is also created, just in case it may be needed in future applications
* The dictionary is made up of key/value pairs of everything in the netstat –i output for each Ethernet interface.  Note: only the packets sent/errors were needed for this application, but storing it as a dictionary makes the other data accessible if ever needed like receive packets, receive errors, etc.
* My interest was in monitoring Eth0 – so this application is specifically tracking Eth0’s total packets sent.  Remember the purpose is to track errors, but to simulate I’m using the total packets.  Since the same dictionary has the key/value pair for errors, it would be extremely easy to change.
* If there are more than 3 packets (read errors) sent in the last 5 seconds, a trigger is sent to the server.
* Due to the default nature of sockets in Python, the data sent over the socket needs to be sent as a string.  Ideally, I wanted to send as a dictionary, but this isn’t possible, so you’ll notice I sent it as a string.
* In a future version, I may look into using a JSON-RPC library, so the data can be serialized and sent over the wire in JSON and this will eliminate the funkiness of what I’m doing today.
* Then the server receives the data over the socket.  The data is received as a string, but then it is parsed back into a dictionary (funkiness) and output it in JSON format on the server.
* The process repeats every time there are more than 3 packets sent in a 5 second interval.

I’m on a loaner laptop and don’t have access to Camtasia right now to create a video, so here are some screen shots of the client and server output.  

Note: The slides also have the code used to create this.  The code isn't very easy to read in the slides, but my blog platform is making it very difficult to post source code.  Hope to update it soon.  

SLIDES MOVED TO [SLIDESHARE](http://www.slideshare.net/jedelman99/example-python-script-for-nexus-3k)

I’ll never claim my code to be perfect or best practice, since I’m learning as I go, but feel free to comment below and let me know what you think about this or more generally about agent based network monitoring and analytics tools running on network devices.

By the way, this testing was done a server and will be tested soon on Arista EOS and hopefully Cumulus Linux.

Thanks,
Jason

Twitter:  @jedelman8



