---
layout: post
tags: arista, api
title: Arista's Programmability Strategy
---

Arista is largely known for its operating system, best known as EOS.  Arista has been known to deploy new features at a more rapid pace than other vendors and to have a more open OS--since EOS was the first production-grade network network operating system to expose any form of Linux to end users.

Because of this, I believe it's perceived Arista has a better programmability strategy than other vendors.  From what I can tell, it is not the case.  However, given a few features Arista has in EOS, it makes programming EOS a bit easier than other platforms.  Let's take a look.

At Network Field Day 16, Arista reviewed their programmability strategy.  There were 5 core components reviewed:

  1. EAPI
  2. OpenConfig
  3. NetDB Streaming
  4. Turbines
  5. EosSdk


![Arista Programmability Strategy](/img/arista-nfd16.PNG)


Before diving into each of these, I'll first point out that when I look at "OS programmability," what is important [to me] is device-level programmability (not controllers or streaming capabilities--those are important topics, but should be covered on their own).  Programmability is the ability to _program_ change on a device, isn't it?  Now let's look at the 5 components in Arista's strategy.

**EAPI** - it's a great API for learning to program an EOS switch.  This is on par with Cisco's Nexus NX-API CLI.  However, EAPI (same for NX-API) is not a robust RESTful API.  They are great APIs for learning because they still use commands.  It's the kind of API you need when the OS was built before an API-first strategy, which is totally fine (but shouldn't be the final _strategy_).  You simply send commands to the device via HTTP/S and get JSON responses back (if the command is supported).

**OpenConfig (OC)** - on its own, OC should not be a programmability strategy without more context provided.  What transports are supported? Is it streaming only?  Is it for configuration management?  What models are supported?  In Arista's case, this is gRPC only and for streaming data only.

**NetDB Streaming** - I'm hard-pressed to include this one for two reasons. It's for streaming (not _programming_) and you need to engage with Arista as a customer to understand it / use it in more detail.

**Turbines** - as you can see from the photo, these are custom apps on top of CVP.  As mentioned previously, I'd rather not conflate device level programmability and _controller_ programmability.  In addition, it's still a _work in progress_.

**EosSdk** - it is in fact an avenue for device level programmability, but for the  control plane of the switch (compared to management plane).  I think this is slick and great knowing it's there, but out of reach and not needed for the majority of EOS users.

Update: shortly after this published, I was informed the latest release of EOS has gRPC support for streaming and also configuration of certain models.  Some of these models are also exposed via NETCONF too.

# Summary

When I take a step back and look at Arista's strategy for programmability, this is what I'm left with:

1. Arista should consider creating a real object-based RESTful API--one that doesn't use CLI commands, even if this uses custom models.  RESTCONF comes to mind.
2. There needs to be a differentiation between programmability and streaming telemetry.
3. Be very explicit with what's supported with regards to OpenConfig.  See above for more context.
4. If a platform is supporting YANG models, it would be much preferred to support NETCONF and/or RESTCONF for a real API (in the context of configuring the management plane).  These are preferred over gRPC for the majority of users at this point in time.  For example, in Python it's quite easy to get started with Python `requests` and `ncclient`.  How would one would get started with a vendor-neutral gRPC client?  From what I've seen, every vendor has been developing their own gRPC clients thus far.  Does this mean it's not standard gRPC?

What makes eAPI a valid strategy (and more programmable than other OSs) is not the API itself, but EOS supporting two features: config replace (atomic config replace) and configuration sessions (batch transactions like a candidate configuration).  Having an open OS and these features are great, but shouldn't minimize the focus on proper [configuration] API development.

General thoughts?  Feel free to comment below!

Thanks,

Jason (@jedelman8)





