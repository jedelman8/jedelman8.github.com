---
layout: post
tags: nfd, code, open source
title: From CLI to API at Networking Field Day
---

The industry is in a shift from the CLI to the API, from manual to automated, and from closed to open.  While some vendors just say they have an API, some provide libraries and tooling to make it easier to consume their APIs.  This post specifically highlights open source code that is publicly available on GitHub by vendors that participated in [Networking Field Day 10](http://techfieldday.com/event/nfd10/).

Please realize this is not an extensive list, but only what is relevant to the specific products covered in the sessions at Networking Field Day.  In order of their presentations...

## Cisco

The APIC-EM, used as part of the IWAN solution, has a full REST API.  No SDK or libraries were mentioned, but it doesn't seem like it's officially shipping yet anyway --- more details can be found [here](https://developer.cisco.com/site/apic-em/documents/api-reference/) on the APIC-EM.

## Big Switch

Both of Big Switch's controller platforms have complete REST APIs.  You can find some code examples here: [https://github.com/bigswitch/sample-scripts/tree/master/bcf/webinar](https://github.com/bigswitch/sample-scripts/tree/master/bcf/webinar)

## Riverbed

Riverbed also talked quite a bit about their APIs across the SteelHead product suite.  You can find plenty of Python libraries on their GitHub page.  You can get started here: [https://github.com/riverbed/steelscript](https://github.com/riverbed/steelscript)

## Gigamon

Gigamon also released REST APIs so that users can programmatically control which flows, etc. are redirected to the proper tools.  Unfortunately, I couldn't even find a GitHub site for Gigamon.  I do believe their REST API is fairly new, so maybe we'll see some code and libs from them soon.

## Intel

There was a great discussion from Intel on the Data Plane Development Kit (DPDK).  It doesn't seem to be on GitHub, but it's still public, and can be cloned should you be a hardcore developer and want to take a look :).  More information can be found here: [http://dpdk.org/dev](http://dpdk.org/dev)

## Arista

Arista has some solid integrations, notably Python and Ruby libraries that simplify working with eapi.  They can be found here: [https://github.com/arista-eosplus/pyeapi](https://github.com/arista-eosplus/pyeapi) and [https://github.com/arista-eosplus/rbeapi](https://github.com/arista-eosplus/rbeapi).

Additionally, they have Ansible and Puppet integrations that can be found on the EOS+ github site: [https://github.com/arista-eosplus](https://github.com/arista-eosplus).

## Nuage

Nuage formally announced the VSPK, which is a Python SDK for working with the VSP Platform. It was awesome to see how pumped they were announcing this!  The code can be found here: [https://github.com/nuagenetworks/vspk](https://github.com/nuagenetworks/vspk)

## Juniper

Juniper mentioned a lot with regards to APIs and programmability.  The best thing about Juniper is ALL of their devices have a common NETCONF API (some are also exposing a REST API too).  They've had a library for sometime now and it's always been public - it can be found here: [https://github.com/Juniper/py-junos-eznc](https://github.com/Juniper/py-junos-eznc).  Additionally, they have Ansible modules as well: [https://github.com/Juniper/ansible-junos-stdlib](https://github.com/Juniper/ansible-junos-stdlib)

OpenClos was announced last year, but also talked about today.  It is a Python library that helps automate the design, deployment, and maintenance of a Layer 3 IP fabric built on BGP (of course, using Juniper kit).  It's also public and be found here: [https://github.com/Juniper/OpenClos](https://github.com/Juniper/OpenClos)

Contrail was also covered and you can find a ton of GitHub projects on Contrail (remember Contrail itself is open source too): [https://github.com/Juniper](https://github.com/Juniper)

## Closing

As you can see, APIs or code made it into **EVERY** presentation at [Networking Field Day 10](http://techfieldday.com/event/nfd10/)! Times are changing --- let the good times roll!


Thanks,
Jason

Twitter: @jedelman8

