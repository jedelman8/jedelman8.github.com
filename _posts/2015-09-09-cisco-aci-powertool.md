---
layout: post
tags: cisco, aci, automation, powershell
title: Cisco ACI PowerTool
---

If you are frequent reader of this blog, it's no surprise I'm focused on automation these days.  It's been primarily centered around using Python and Ansible with a little Puppet and Chef sprinkled in.  I had the opportunity recently to change things up a bit using the Cisco ACI PowerTool and thought I'd share a few things about it.

First off, the ACI PowerTool is a PowerShell module that helps automate all aspects of a Cisco ACI fabric.

Second, it's no a secret that the same [rocket scientist](https://twitter.com/dvorkinista) created both the Cisco UCS and ACI object models.  That said, the UCS PowerTool has been around for years and offers PowerShell modules that can be used to manage, operate, and automate Cisco UCS environments.  As you may have guessed the Cisco ACI PowerTool is the same thing, but used to manage and automate Cisco ACI fabrics using PowerShell.

And as luck would have it, I'm still a Windows user, so I was able to get this up and running extremely fast.  In full transparency, I haven't spent much time with PowerShell at all before this, and it was super easy to get going, so no matter what your background, it's worth checking out --- even if that means you are a hardcore network engineer!

# Getting Started

## Download and Install the Cisco ACI PowerTool

The msi can be found at the Cisco Communities site [here](https://communities.cisco.com/docs/DOC-55812).

Note: if your existing version of PowerShell is incompatible with what is required (v4), the installation will stop.  If this happens, as it did to me, just Google for the correct version and download it from Microsoft.  The downside if this happens:  a reboot is required to continue on.

## Download the Cisco ACI User Guide

The user guide and is pretty clear and straight forward giving some great examples.  This is also on the communities site and can be downloaded [here](https://communities.cisco.com/docs/DOC-55747).

## Get a working ACI Fabric

The ACI lab in [dCloud](https://dcloud-rtp-web-1.cisco.com/dCloud/) works **GREAT** for testing.

## Begin Testing

First, launch the Cisco ACI PowerTool that was just installed.

You'll see the following screen.

![p1](/img/acipt/acipt1.png)

As you can see, there are 4 four helpful _Cmdlets_ that Cisco shows that you can use right away.  We will try a few of them out.

Connect to the ACI Fabric using the `Connect-Aci` Cmdlet.

![p2](/img/acipt/acipt2.png)

At this point, we are now logged into the fabric, and can perform more tasks in real-time from the PowerTool interactive shell.

We'll now stop using screen shots and use raw text to show how to check out all configured tenants on the system.

```
PowerTool C:\> Get-AciFvTenant


Descr        :
LcOwn        : local
ModTs        : 2015-09-09T20:31:44.124+00:00
MonPolDn     : uni/tn-common/monepg-default
Name         : infra
OwnerKey     :
OwnerTag     :
Uid          : 0
Dn           : uni/tn-infra
Rn           : tn-infra
Status       :
XtraProperty : {}

Descr        :
LcOwn        : local
ModTs        : 2015-09-09T20:31:18.344+00:00
MonPolDn     : uni/tn-common/monepg-default
Name         : common
OwnerKey     :
OwnerTag     :
Uid          : 0
Dn           : uni/tn-common
Rn           : tn-common
Status       :
XtraProperty : {}

Descr        :
LcOwn        : local
ModTs        : 2015-09-09T20:31:46.438+00:00
MonPolDn     : uni/tn-common/monepg-default
Name         : mgmt
OwnerKey     :
OwnerTag     :
Uid          : 0
Dn           : uni/tn-mgmt
Rn           : tn-mgmt
Status       :
XtraProperty : {}

```

Maybe you want to add a new tenant now.  This can be done with the `Add-AcifvTenant` cmdlet.  

Here is an example:

```
PowerTool C:\> Add-AciFvTenant -name "new_tenant"


Descr        :
LcOwn        : local
ModTs        : 2015-09-09T20:54:53.922+00:00
MonPolDn     :
Name         : new_tenant
OwnerKey     :
OwnerTag     :
Uid          : 15374
Dn           : uni/tn-new_tenant
Rn           : tn-new_tenant
Status       : created
XtraProperty : {}


```

And for checks and balances, let's ensure it's returned in the `Get-AciFvTenant` Cmdlet we ran previously.

```
PowerTool C:\> Get-AciFvTenant


Descr        :
LcOwn        : local
ModTs        : 2015-09-09T20:31:44.124+00:00
MonPolDn     : uni/tn-common/monepg-default
Name         : infra
OwnerKey     :
OwnerTag     :
Uid          : 0
Dn           : uni/tn-infra
Rn           : tn-infra
Status       :
XtraProperty : {}

Descr        :
LcOwn        : local
ModTs        : 2015-09-09T20:31:18.344+00:00
MonPolDn     : uni/tn-common/monepg-default
Name         : common
OwnerKey     :
OwnerTag     :
Uid          : 0
Dn           : uni/tn-common
Rn           : tn-common
Status       :
XtraProperty : {}

Descr        :
LcOwn        : local
ModTs        : 2015-09-09T20:31:46.438+00:00
MonPolDn     : uni/tn-common/monepg-default
Name         : mgmt
OwnerKey     :
OwnerTag     :
Uid          : 0
Dn           : uni/tn-mgmt
Rn           : tn-mgmt
Status       :
XtraProperty : {}

Descr        :
LcOwn        : local
ModTs        : 2015-09-09T20:54:53.956+00:00
MonPolDn     : uni/tn-common/monepg-default
Name         : new_tenant
OwnerKey     :
OwnerTag     :
Uid          : 15374
Dn           : uni/tn-new_tenant
Rn           : tn-new_tenant
Status       :
XtraProperty : {}




```

Luckily it is there, so now we know it is working.

There is plenty more than can be done with the PowerTool to automate ACI --- you should definitely check out the [user guide](https://communities.cisco.com/docs/DOC-55747).  This was merely meant to be a short introduction.  

Of course, [Ansible](https://github.com/jedelman8/aci-ansible) could also be used, but if your team is already using PowerShell for other tasks that span compute and virtualization, it's worth taking a look at all of the available options.

Thanks,
Jason

Twitter: @jedelman8
