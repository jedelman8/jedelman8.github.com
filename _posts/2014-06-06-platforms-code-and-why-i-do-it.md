---
layout: post
title: Platforms, Code, and Why I do it
tags: code
---

If you read this site often, you already know I’ve been doing quite a bit of work with Ansible specifically as it pertains to networking.  While I will be showing another video very soon in a follow up post, I wanted to take a step back and cover a few things before doing so.  The focus here is less about the technology and more my general mindset around automation PLATFORMS, code, open source, and why I do it.  Just something I’d like to share because I’m occasionally asked questions around these topics.

### Culture 

It’s not about the tool, in my case Ansible, it’s about the process, methodology, and ideas that go into thinking differently.  For me personally, Ansible just had a lower barrier for entry, but I’ve grown to quite like it.  Actually, I liked it from the get-go.   In order to effectively embrace platforms like Ansible, there needs to be a change in [culture first](http://dev2ops.org/2013/09/john-willis-notes-notable-devops-culture-traits/).  It was amazing to see the emphasis on culture during the recent [DevOps days in Pittsburgh](http://new.livestream.com/accounts/1466347/events/3044568/?utm_source=hootsuite&utm_campaign=hootsuite).  I tuned in and out during the live stream and it seemed every time I had a chance to watch, it was all about culture.  I feel like my mind/culture is already changed, hence the focus here.

### Continuous Learning

Using Puppet, Chef, Ansible, or any of the other tools out there is good, but another reason I like them is from an efficiency of learning and time management perspective.  To build a [custom module in Ansible](http://docs.ansible.com/developing_modules.html), you will likely use Python.  Other tools are based on Ruby.  This means learning some common DevOps tools and Python/Ruby at the same time!  How’s that for time management? :)

### Example Why Every Engineer Won't Need to Program

After creating a few custom modules (or equivalent), you can see that it’s quite possible to have different engineers/admins with different skillsets leveraging and using these platforms.  It’s fully possible to have one person execute Playbooks, another person architect the Playbooks (using core modules) so they are fully relevant for the environments they support every day, and then have another person that builds custom modules based on defined requirements.  The only person that needs to be familiar with code would be the person building custom modules.  Why does EVERY engineer need to code again? :)

That all said, I’m of the belief, the more you know, the better. 

### Why Program? 

More specifically, why have I personally started down this path of this thing we call “code.”  First off, as much as I’ve done, I still have so much more to go, but the journey has been amazing so far, and I’m definitely going to continue.  But really, why?

**Satisfaction** – I’m a hard person to satisfy.  Building products, creating products, and seeing your work in action is gratifying.  I truly do enjoy it.

**Make an Impact** – The time is now where not only companies, but also individuals, can come together and create a community, which can have an impact, and help shape the industry.  That’s what I believe anyway.

**Give Back** – How many blogs have you read?  How many videos have you watched soaking up information?  It feels good to give back.

**We need better tools** – okay, why do we need better tools?  Well, what tools do we really have today?  The joke is, well we have ping and traceroute, right?  Many users have your basic polling engine and some have more advanced tools, and others have home-grown solutions.  For the commercial tools, if you needed to add a custom widget to the tool, can you?  Just a minor tweak, even.  Do you need to wait until next release cycle? For the home-grown solution, how is it supported?  Are you reliant on one or two key engineers?

**It’s different now** - The open source movement from a tooling perspective has given me hope on what tools can be developed purpose built for the network industry.  Imagine going to the community to get assistance building modules, giving back, and having a solid platform that can be built on.  Stop, pause, and really imagine it. 

And how many existing tools today are true platforms?  The only platforms we have coming are SDN controllers --- we still need better tools.

### Bringing it home...

While there are several reasons why I do what I do when it comes to development, I’m sure there are more, but what it boils down to is this statement I recently read by Matt Oswalt, “The second is a network engineer that – if for no other reason – writes code because they truly want to, not because they feel like they have to…”  I couldn't agree more – you have to WANT to do it.

Check out Matt's full post on [The Evolution of Network Programmability](http://keepingitclassless.net/2014/05/evolution-network-programmability/).  Definitely worth a read.

Check back soon for the video mentioned in the intro paragraph.  I’ll be showing how to use Ansible for automating and validating OSPF configurations.

Thanks,
Jason

Twitter:  @jedelman8

Links to my Ansible posts:

[Ansible For Networking](/home/ansible-for-networking)
[Network Test Automation with Ansible](/home/network-test-automation-with-ansible)
[Video: Using Ansible for Network Automation](/home/demo-using-ansible-for-network-automation)
