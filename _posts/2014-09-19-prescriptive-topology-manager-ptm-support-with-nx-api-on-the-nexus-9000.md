---
layout: post
title: Prescriptive Topology Manager (PTM) support with NX-API on the Nexus 9000?
tags: ptm, cumulus, nxapi, python
---

Cumulus Networks has been talking a lot about Prescriptive Topology Manager (PTM).  A great overview of PTM can be found [here](http://cumulusnetworks.com/blog/complex-topology-and-wiring-validation-in-data-centers/), but the high level is that PTM ensures “wiring rules are followed by doing a simple runtime verification of connectivity.”  This means that as a user, you can define what the physical topology, or wiring, is supposed to be and compate it against what it really is by leveraging LLDP.  The PTM daemon (PTMd) is what does this analysis on each switch running Cumulus Linux.  There is even integration with routing protocols such that if two switches are improperly cabled, no routing adjacencies will be permitted on that link.  You can check out the PTM [code](https://github.com/CumulusNetworks/ptm) since it is available under the Eclipse Public License (EPL).

Cumulus is said to have a few, but very large customers --- these customers operate at the highest efficiency levels and it is customers like these (speculating here, just go with me) that probably drove Cumulus to develop a feature like this.  However, this is a real problem for networks of all sizes.  I’ve seen 100s to 1000s of pages of word docs and excel sheets that mapped out exactly what each port should be connecting to.  While not every device supports LLDP (or CDP), there are MANY that do.  While Cumulus is totally focused on the data center, the same methodology and approach could be used across the campus validating phones, access points, video units, uplinks, vSwitches, etc.  I can recall times I’ve seen engineers working in union buildings where the union didn’t follow the patch schedule (wiring scheme) provided, and troubleshooting and fixing the cabling was a pure nightmare.  Imagine waiting 20 minutes for an elevator in a large pre-construction building just to go to the 30th floor to fix a few cables and then have to do that 3 or 4 more times in the same day!  This is reality.

The problem PTM is solving seems so obvious, but are any other vendors supporting PTMd on their switches?  After all, it’s Linux, and most switches are running Linux, so what’s holding them back?  Unfortunately, it’s still not that simple due to feature delivery, product management and development, internal processes, etc., but if anyone does have information of any vendor planning support for PTMd, I’d love to know.  Write it and let me know!

### Okay, so how does the Nexus 9000 support PTM?

In theory, it doesn’t.  But, there is no reason the concepts of PTM cannot be applied offline by comparing active run time data via LLDP/CDP with a prescriptive topology document that was put together by a network engineer.  PTM works by describing the topology in a [graph description language][1] – a text file following the language that ends with the file extension .dot.

[1]: http://en.wikipedia.org/wiki/DOT_(graph_description_language)

While I’m sure it would be possible to develop a dot file for what I did, I chose to use another description language that is already well known (it’s just not a graph, so there is some redundancy built into the file).  I chose to implement this in YAML.  YAML is becoming more popular these days and I’ve written about it before because it’s the same description language that Ansible uses to define their playbooks.

### The Example

While I have usually been integrating this kind of script into Ansible, this time, and for this post, I’m trying to emphasize more on the outcome, but if you end up wondering as you read this post, the short answer is yes, the Python script that I’ve built can easily be ported over to any DevOps automation framework.  Okay, back to the example…

The following diagram depicts the current physical setup that I’ll be verifying.  

![diagram](/img/ptm1.png)

Note: the crazy cabling scheme does not depict a real world cable design.  I just wanted a distinct and different interface per switch to make it very easy to verify where the cabling error was occurring in the output for demos.

Now that you’ve seen the topology, here is a look at the YAML file that describes this physical topology.

![cable-desired](/img/ptm2.png)

If you haven’t seen YAML before, you can see here that is very human friendly, but at the same time machine readable. 

In the following test, the Python script gathers active CDP data from both Nexus 9000 switches using the NX-API and compares it to what is described in the YAML file.  The filename is topo.yml.

After a run of the script, here is output.  As you can see, all is cabled properly!

![cabling](/img/ptm3.png)

Now I’ll modify the YAML file to see what happens when the prescribed topology doesn’t match the active state of the device.  Note the change here - changing port 22 to port 21:

![error](/img/ptm4.png)

Let’s re-run the script and see the output.

![output](/img/ptm5.png)

Here we see the prescribed topology (what it SHOULD be) does not match the active run-time state.

### Two other scenarios and screen shots to show:

A connection is described in the topo.yml, but is not found in the active CDP tables.  A non-existent connection was added to the YAML file to perform this test.

![sce1](/img/ptm6.png)

The next test removed an interface from the working topo.yml file, but it’s still found in the active CDP table. In this scenario, we may want to deny any L3 from occurring over this link.

![sce2](/img/ptm7.png)

I will re-iterate that this can be easily integrated to DevOps tooling to further react based on the result found to do things like shutdown interfaces, prevent L3 routing adjacencies, and actually have a pretty robust workflow.  FWIW, this was less than a day of development and that even included commenting and one re-factoring of the code.

From a comparison perspective, remember PTMd runs locally on the device as a daemon so there is no need for an external script or DevOps tool integration and it’s just “part of Cumulus Linux”….but, even Cumulus recommends deploying the .dot files via a DevOps tool, so there could be really interesting integrations all around for PTM-like functionality going forward.  Huge thanks to Cumulus for open sourcing PTM.

We must remember that we don’t necessarily need a feature ON THE BOX too.  In fact, it may be more scalable and be better off being off box to support 3rd party and multi-vendor environments in certain situations.

As always, I’d love feedback.  Would this be helpful for you?  What types of issues have you run into that this may have prevented?

*Note: I haven’t posted this code yet to GitHub, but may do that very soon.*

Thanks,
Jason

Twitter: @jedelman8