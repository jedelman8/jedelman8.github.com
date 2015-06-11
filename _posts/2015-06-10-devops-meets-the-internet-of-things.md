---
layout: post
tags: iot, devops
title: DevOps Meets the Internet of Things
---

When I initially heard about the Internet of Things (IoT) sometime in the past few years, my initial reaction was okay here we go, we have another buzz word that means absolutely nothing.  Add in Internet of Everything (IoE), it seemed even worse.  After spending some time participating in an IoT Hackathon this past weekend in the [DevNet Zone](http://www.ciscolive.com/us/activities/devnet-zone/) at Cisco Live, I can honestly say that my opinion has changed.  Here’s why.

## Background
I was set to arrive at Cisco Live on Saturday to attend a DevOps forum on Sunday, but after booking travel and continuing to browse the Cisco Live website, I found out they were having an Internet of Things hackathon that would be starting on Saturday, go through the night, and finish on Sunday.  It seemed intriguing because around the same time a highly valued peer of mine had just been telling me about a Cisco device that is still in beta, codename doublemint (more on this later), that is helping consume and deploy IoT-enabled devices.  Now I needed to dig in and try to attend the hackathon.  Being that I was set to arrive after the hackathon was to start, I emailed the DevNet team making sure it was okay to still pop in and hack.  Sure enough, it was fine.

## Hackathon

By 3:30pm on Saturday I finally got in and settled at the hackathon.  Mike Maas from Cisco greeted me and had me setup with the proper equipment within a few minutes (to give you an idea, [check this out](https://developer.cisco.com/site/eiot/documents/quick-start/))  The equipment that I used included a Cisco Talem device (aka code name doublemint) and several [phidgets](http://www.phidgets.com/).  The Talem/doublemint device is a brick (think POE brick) that is powered via POE, but that can then in turn power a USB connected device.  Pretty slick.  Not only that, it is essentially extending the USB bus of the device over the network.  *Note: a special version of IOS is required on the Catalyst switch that is used to power a Talem device.*  In order to write a script that could communicate with the USB device that is connected to the Talem, I needed to install the [Cisco version](https://developer.cisco.com/site/eiot/documents/pyusb-dev-guide/) of the python library called `pyusb`.  I installed this on my local machine – what happens then is my script connects to the local Catalyst switch and sends/receives USB commands (via the switch transparently) to communicate with the USB device connected to the Talem.  Make sense so far?  

## What about these phidgets? 

These phidgets are basically USB enabled sensors (well, sensors that connect to a USB adapter). There are many kinds as can be seen from their website.  I started with using pressure and light sensors.  Within minutes, I was able to use sample scripts Cisco provided to start reading data from these USB sensors.  It was fascinating to see how straight forward it was --- granted Cisco had done the heavy lifting providing us some [sample scripts](https://github.com/CiscoDevNet/eiot-example) :).  Thanks to Mike Maas here again.  *Note: more than phidgets can be used here.  Another prime use case for the Talem device is to power [iBeacons](http://store.twocanoes.com/products/bleu-beacon-with-ibeacon-technology-single-pack?utm_medium=cpc&utm_source=googlepla&variant=1071178061&gclid=CObysIu-hsYCFY2RHwodd4YAqQ) that are deployed throughout retail locations eliminating the need to run power everywhere a beacon is deployed.*

## Mike Aossey Jumps In!

By 5pm, I was contemplating entering the hackathon after looking at a few other phidgets and hearing there were only about 10 teams with a grand prize of $10K.  The only issue was a team of two or more was required.  I sent [Mike Aossey](https://twitter.com/aossey) a message on Twitter asking if he was interested and within the hour, he was there, and we started developing what we considered a simple, but yet an effective and practical application for Enterprise Security.  The application we were hoping to develop would integrate IoT with DevOps, mainly Network Automation, to enhance the security of a Campus Network.

## BYOD Security with Biometrics

If you look at the security solutions that serve the market today for network access control, they usually involve passwords, certificates, tokens, and a number of other ways to validate who the user is.  Since fingerprint scanning is already widely used for device access, could it also be used for network access?  Absolutely.  That’s what we set out to do.  The only issue was we didn’t have a fingerprint scanner.

## Getting Creative

Since a fingerprint scanner wasn’t available, our plan was to use the pressure sensor to simulate a fingerprint scanner.  A low pressure would simulate person A and a higher pressure would simulate person B.  In reality, the pressure sensor returned values from 0 – 800, so we could have simulated more than two people, but it was difficult to re-produce the same pressure, so we just used a single threshold value when the pressure was greater than zero.  

We had values being returned to our script from the sensor – the next step was then to trigger dynamic VLAN assignment based on these values (could have also done ACLs or any other switch/device configuration).  Rather than re-invent the wheel, we used Ansible for this since I already had [Ansible Tower](http://www.ansible.com/tower) setup on my laptop.  We created a new Ansible playbook and Job Template in Tower that accepted the pressure value and interface as two [extra variables](https://docs.ansible.com/playbooks_variables.html#passing-variables-on-the-command-line).  Based on the pressure, a different VLAN was getting assigned to the switch.  Since we were using Tower and not just Ansible CLI, we were easily able to call the Tower REST API to kick off the job template and get the VLAN assigned.  It was pretty sweet.  The end workflow we were after was this:  interface in a default/quarantine VLAN, user opens web portal and enters their office/cube location while pressing the sensor (ala fingerprint scanner), and based on who they are, they port gets configured with the correct policy.

**Note: based on how we did things, we could have easily used any sensor to trigger any type of configuration on the switch.**

Everything ended up working as expected (sort of!) – we just ran out of time to get a pretty front end developed since we did NOT pull an all-nighter!  We chose to get to sleep by 12:30am on Saturday, but we were right back at it by 8am on Sunday and were testing straight through until the hackathon finished at 1pm.  There was some mishap on Sunday that didn’t have us in the formal competition, but overall it was still a great experience, and I think speaking on behalf of both of us, we learned a ton.

## Looking into the Future

Going forward, I’m hoping to squeeze in more time for things like this because it really shows the true value of programmability and integration between disparate systems, tools, and technologies and could solve real-world problems.  The great part is those who are looking to ramp up on development skills as it pertains to network automation and programmability will likely be able to use those same skills in the future in areas that we can’t even fathom yet as technology and IoT continues to gain traction in the market.

Thanks,
Jason

Twitter: @jedelman8

