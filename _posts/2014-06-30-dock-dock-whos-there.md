---
layout: post
title: Dock Dock.  Who's there?
tags: docker
---

In my previous [post](/home/docker-networking) about Docker, I focused on an introduction to networking with Docker.  That post had a fair amount of traction mainly due to it being #dockercon the week it was published, and seemingly, people had an interest in learning more about it.  Following the post, there were a few folks (@hartley and others) that pointed me to some great [links](https://twitter.com/hartley/status/478638933430263808) about more advanced concepts in Docker and a [site](http://blog.thestateofme.com/2014/06/08/connecting-docker-containers-between-vms-with-vxlan/) that validated what I was speculating with leveraging overlay tunnels as means for connectivity between nodes running Docker.

After reading through a few posts and the links about [libswarm](https://github.com/docker/libswarm) and [libchan](https://github.com/docker/libchan), it was on my mental to-do list to learn more about them and test more advanced designs.  In the meantime, the friendly folks at Digital Ocean let me know about a local meetup where James Turnbull, VP of Services at Docker, was presenting.  The meetup was tonight.  It was a short presentation, but for me, it was eye opening (it may be because I’ve been *mostly* focused on networking).  So, you won’t find a deep dive here like the last post, but rather, some general thoughts on Docker motivated from the meetup.

* The sexiness of Docker is around orchestration as was conveyed tonight.  It’s about portability.  It’s about making it insanely simple for developers to do their thing and write code without worrying how it’ll get ported over to production.  It’s about the simplicity of a workflow that ports a container from any QA/DEV environment into production.  Production could be an on-prem bare metal machine or a VM located in AWS or Digital Ocean.   What’s interesting to me is the “sell on portability” as this has been the goal for Hybrid Cloud or multi-cloud for a while now, but leveraging virtual machines, instead of containers.  If you ask me, Docker could be what we’ve all be hoping for when it comes to workload mobility.
* A caveat though --- Docker is operating system virtualization and containers are meant to be a home for a single app or process, nothing more.  I’m sure we’ve all heard that before, but the effect or the reasoning I heard tonight was new (at least for me).  It was about having a single process per container that made operations extremely simple.  Is the process up? Is it down? Does it need to be restarted?  That’s it.  Troubleshooting becomes streamlined because the end users are not troubleshooting a monolithic application and fault domains become extremely small from an app perspective.  Expanding on the caveat – how many Enterprise apps would even be good candidates for containerization?
* After the session, I had a chance to talk to James Turnbull for a few minutes and talked about networking, of course.   As stated in my last post, it’s completely possible to leverage the networking stack and existing OVS that already exists on the host.  I’ll be honest – when I read about libchan and libswarm initially over a week ago, it didn’t sink in.  I picked one tonight and asked about it.  It was about libchan and as I asked questions, I used analogies to overlay networking protocols, such as VXLAN.  While I’ll probably end up reading about libchan more later this week, the takeaway example for now is this, if there is an app container that needs to talk to a db container, they only need to communicate over particular ports/protocols.  Today, this policy may be hard to maintain, define, manage, and deploy. 
* With Docker, this communication would happen over a libchan connection that would get nailed up between the two containers.  Seems like another overlay if you ask me.  The question was raised, “why would the app developers want to deal with the security team to get the proper security?” [Will infrastructure really becomes a utility](http://blog.ipspace.net/2013/04/they-want-networking-to-be-utility-lets.html) with Docker?  Definitely need to dig in more, but the opportunities for true flexibility seem possible in this architecture.  And ideally, each host is assigned a FQDN while the subnet, VLAN, or IP really has no bearing on policy (as we are already seeing with applying policy with virtual machine attributes today).
* OpenStack integration was also mentioned briefly.  To integrate with OpenStack, it will be possible to have nova spin up containers or leverage heat orchestration to do this as part of the application deployment workflow.
* When the slide (below) came up, it was said many of the companies (except the bottom tier) were all pretty young, like in the 6 month range, and they are focused on providing orchestration layer capabilities for Docker containers.  Docker is about a year old and they already have several really young companies racing to be the orchestration layer ([or OS as Michael Bushong](https://twitter.com/mbushong/status/478268311415689216)) usually points out).  Imagine this ecosystem in another 1-2 years.  There is already a [startup cloud provider](https://www.tutum.co/) providing Docker containers as a service.  And yeah, [AWS is their infrastructure](https://docs.tutum.co/technical/#what-kind-of-infrastructure-does-tutum-run-on).

![dockdock](/img/dockdock.png)

The impact on infrastructure and the incumbent ecosystems could be significant, and a little more than the hype that we’ve seen with other technologies over the past few years :).

Still need to get more details on libchan and libswarm.  Stay tuned.

If anything is misstated above, please let me know and write in, and I’ll correct ASAP.

Thanks,
Jason

Twitter: jedelman8


