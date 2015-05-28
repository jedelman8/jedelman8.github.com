---
layout: post
tags: aci, cisco, sdn, python
title: Programming an ACI Fabric
---

By now, you've probably heard of Cisco's Software Defined Networking (SDN) solution that is centered around ACI, or the Application Centric Infrastructure.  Like most SDN platforms, a key component is the controller otherwise known as the Application Policy Infrastructure Controller (APIC) in the case of ACI.  The APIC provides a single pane of glass that centralizes policy, configuration, and monitoring of the complete fabric.  It also more importantly exposes the complete system via an object oriented REST API, which is what we'll look at in this post.

By itself, ACI reduces the number of touch points in the network.  This is no different than any other controller-based network that exists today and is a great step in the right direction.  We can then honestly say SDN simplifies operations.

The issue is if you don't have *something else* driving ACI, or any other SDN solution for that matter, it could take a large number clicks within the UI to configure a  new tenant, application, or whatever is being configured.  This is error prone as we all know it's pretty easy to fat finger something!  Because of this, it still makes total sense to automate network fabrics even if it's not for a cloud deployment.

> Note: Something else *driving* a SDN solution could be a cloud management platform, automation tool, custom script, etc.  

So, what is most intriguing about ACI is how Cisco is lowering the barrier to entry for network engineers and developers to take advantage of the fabric by using the APIC APIs -- the same APIs that the web GUI is using.

#### How so?  How is Cisco making it easier to program and communicate with the REST APIs?

The answer consists of four tools Cisco has provided:

1. APIC REST to pYthon Adapter (known as ARYA)
2. API Inspector
3. Managed Object Browser - Visore
4. ACI SDK - Cobra

Here is a very brief description of each.

## ACI SDK - Cobra

First and foremost, Cisco provides a Python SDK that streamlines the process of making API calls to the ACI fabric.  Cobra maps directly to the object model, so if you understand that, then Cobra will make much more sense.  

Logging into the fabric via APIC is the first thing that always needs to happen.  This can be accomplished like so:

```
session = LoginSession(apic, username, password)
moDir = MoDirectory(session)
moDir.login()
```

Need to gather all of the tenants on the system?  That looks like this:

```
tenant_objects = moDir.lookupByClass('fvTenant')
```

Some of the naming may look awkward, but that's just the naming within the object model, i.e. mo = managed object, fv = fabric virtualization, etc.

The point is, you don't actually need to worry about the REST calls because this abstraction layer, or SDK, is provided.  

## Managed Object Browser - Visore

Need to verify the object hierarchy within the fabric?  If so, check out Visore.  Maybe you know the tenant object is `fvTenant`, but want to verify what the children of `fvTenant` are.  First you browse to Visore located at `https://apic/visore.html`.

Once you login, you can query based on object.  We'll stick with `fvTenant`.

![apic1](/img/apic1.png)

Once you send the query, all associated objects are returned.  One of them here is a tenant called `ACILab`, which can be seen below. Actually, in this example, I specifically queried for `ACILab` as you can see from the picture.

![apic2](/img/apic2.png)

See the `dn` property?  That is the distinguished name of the object.  To check out the children of `ACILab`, just click the right green arrow and then you'll see a larger number of objects (if it's an active tenant).

![apic3](/img/apic3.png)

The picture is showing 1 of the 11 objects being returned.  This one in particular is an Application Profile.  A Bridge Domain is shown at the bottom, but is cutoff.

> You'll also notice you can *Display URI of last query* to see what the REST call being made is.  This can come in handy especially if you aren't using Cobra.

The process can continue until you find the object or attribute you're looking for.

##  API Inspector

This one is a little more common, but still a great feature.  API Inspector allows you to configure something within the APIC GUI, but allows you to see exactly what API call is being made in real-time.  It's accessible while you're in the GUI at the *welcome, user* drop down in the top right of the UI.

Check it out:

![apic4](/img/apic4.png)

Search for the POST request and we can see the new tenant being added.

![apic5](/img/apic5.png)

Again, very valuable for troubleshooting as you starting communicating with the REST APIs of the fabric.  We'll see API Inspector in action in the next section too.

## ARYA

Arguably what I find most fascinating out of these four tools is ARYA.  There are billions of objects in the ACI object model, which means Cobra is VERY object oriented.  Okay, maybe not billions, but a lot.  ARYA makes it extremely easy to get started with Cobra since it literally generates Python code for you.

Let's take an example.

You want to create a tenant - very simple, single object example.

First, go to the web GUI, open API inspector, and create the tenant:

![apic6](/img/apic6.png)

As you can see, I just entered the tenant name and description.  Then I clicked next and Finish.  As soon as finish is selected, you would see messages in the API Inspector window like this:

![apic7](/img/apic7.png)

We now have the JSON data that was used to create the tenant:

```
{"fvTenant":{"attributes":{"dn":"uni/tn-JEDELMAN_TENANT","name":"JEDELMAN_TENANT","descr":"sample-test-for-blog","rn":"tn-JEDELMAN_TENANT","status":"created"},"children":[]}}
```

This JSON data (XML works too) is fed into ARYA and the output is Python code.  Before generating the code though, the JSON data needs to be stored into a file.  We'll call this `tenant.json`.

Once ARYA is installed, you would execute it like any other Python program.

```
arya.py tenant.json
```

Since there may be some people that don't believe this, I'm including the screen shot and not just the raw text! :)

Check it out:

![apic8](/img/apic8.png)

## Closing

There you have it - an introduction to programming the Cisco ACI fabric.  Many other vendors can take a page out of Cisco's book here.  Don't just say you have an API.  Document and build the proper tools around it to help users adopt, use, and learn about it!

Thanks,
Jason

Twitter: @jedelman8




