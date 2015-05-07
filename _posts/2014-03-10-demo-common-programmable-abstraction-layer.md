---
layout: post
title: Demo - Common Programmable Abstraction Layer
tags: demo, cpal
---

Over the past few weeks, I’ve written about the idea behind a common programmable abstraction layer.  Previous articles are [here](http://www.jedelman.com/1/post/2014/02/common-programmable-abstraction-layer.html) and [here](http://www.jedelman.com/1/post/2014/02/the-power-of-a-programmable-abstraction-layer.html).  It’s worth stating that something like a CPAL can be used with or without SDN controllers and with or without cloud management platforms.  As can be seen from the previous write ups and the video/demo below, today its primary focus is data extraction and data visibility.  It can use device APIs or controller APIs.  It’s about accessing the data you need quicker.  It’s that simple.  No more jumping from device to device and having to manage text and excel files.  

Edit 3/15/2014:
[Github repo for CPAL](https://github.com/jedelman8/cpal)

If there is a controller in the environment, you can still view data around particular physical and virtual switches in the environments by creating the right modules.  Same can be said if there was a CMP/CMS deployed.  While a CPAL can easily make changes to the network, it’s about taking small steps that can have a larger impact on how we use new APIs on network devices and controllers.  And if we don’t strive for a common framework now, we will end up with many more APIs than there are CLIs.  What good is that?

The first step is using these APIs for visibility.  Maybe in the future, the controller or CMP would actually use a CPAL API for provisioning – if not, it’s no big deal because there would still be a common framework for interacting with devices and getting the data needed.

Below is live demo of working with the CPAL.  If you are short on time, I’d ask you to take a look at the last 3 minutes working with tables.  

[YouTube Video](https://www.youtube.com/watch?v=JeaA4MXvd2g)


As always, feedback is welcome.

The code will be posted to git hub soon!

Edit 3/15/2014:
[Github repo for CPAL](https://github.com/jedelman8/cpal)

Thanks,
Jason

Twitter: @jedelman8