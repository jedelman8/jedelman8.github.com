---
layout: post
title: Cisco [Limited] lifetime Warranty Programs
tags: cisco
---

There are a few different warranty programs available from Cisco that are not widely advertised and discussed.  However, given that every customer has different technical and business requirements, it is also true that each customer has different SLAs that have to be met internal to their organization.  So let’s examine some of the options available.

There are standard SMARTnet (SNT) offerings from Cisco that are the norm including 24x7x4 and 8x5xNBD support offerings.  These are probably two of the most popular offerings, but there are many others out there as well.

It is important to note that each of the SMARTnet offerings from Cisco include 24x7x365 access to the Cisco Technical Assistance Center (TAC).  I am not here to promote TAC by any means even though it is a world class organization, but this is questioned by the “8”x”5”xNBD usually found in the description of the SNT SKU being sold.  The 24x7x4 and 8x5xNBD most importantly are referring to the turnaround time to get hardware replacements.  So what if you’re an organization that has 200 user access layer switches, and you already spare a bunch of additional switches?  Good question, and for me personally, I would first move away from 24x7x4 SNT if that was the case.  Since the concern is no longer hardware replacement, the most important piece to the DAY 2 support puzzle becomes having a number to call in the case of a problem.  Again, this is a where TAC comes in and you can leverage 8x5xNBD on the switches.  It would be ideal to do a ROI of sparing vs. different levels of SNT being chosen to see which makes the best financial sense for the organization.  Cisco or your local partner can help you with this.

What if, of the 200 access switches, each will have the same exact configuration, and because it’s a large organization, there is a competent network operations team to diagnose and troubleshoot problems.  You can also work with the Cisco Service Account Manager to look up how many times someone from the Company has called and leveraged TAC over the past 2-3 years.  Make your account team do some work!  This is where the cost and value of TAC does need to be examined every once in awhile and this is the point of this post.

Two of the many Cisco support programs that are always good to analyze are the Cisco Limited Lifetime Warranty (LLW) and the Enhanced Limited Lifetime Warranty (E-LLW) programs.

**LLW** – supplies hardware replacement within 10 business days, while providing NO access to TAC.  Warranty applies to Cisco Catalyst® Express 500, Cisco Catalyst 2960, 2975, 3560, 3560-E, 3750, 3750-E, 4500, and 4500-E Series Switches, 600, 1140, 1260, and 3500 Series Access Point.  Some limitations apply.

**E-LLW** – supplies hardware replacement within 1 business day, while providing 90 days of TAC (8x5).  Warranty applies to Cisco Catalyst 2960-S, 3560-X, 3750-X, 2960-C, and 3560-C Series Switches.  Running IP Services disqualifies 3KX from using E-LLW.

On a daily basis, I see different customers choose different support programs.  It is also common for our (BlueWater customers) to use one of the LLW or E-LLW and combine that with our internal “Cisco certified” TAC/NOC support because it comes at a reduced cost, while still providing the value add of device replacement if there is a hardware defect.

From a WLAN standpoint, given the architecture of the intelligence residing in the controllers, I am usually fine with forgoing SNT on the actual access points, providing a few spares, and leveraging the LLW program for HW replacement.  SMARTnet should always be included for the WLAN Controllers though, and on that note, it should ALWAYS be included on your Core/Distribution level switches.  There is a reason why the 6500 and Nexus switches are not on listed above :).  However, this may begin to change as the new and intelligent edge goes further into the server.

Here are a few links that are worth a read if you’re serious about analyzing your support/SMARTnet strategy with Cisco:

[Warranty Q&A][1]

[3750X & 2960S Warranty Information][2]

[4500 Warranty Information][3]

[Cisco Limited Lifetime Hardware Warranty Terms][4]

[Cisco Enhanced Limited Lifetime Hardware Warranty Terms][5]

[1]: http://www.cisco.com/en/US/prod/warranty_qa_guest.html#~hardware_software
[2]: http://www.cisco.com/en/US/prod/collateral/switches/ps5718/ps10745/product_bulletin_c25-607000.html
[3]: http://www.cisco.com/en/US/prod/collateral/switches/ps5718/ps4324/product_bulletin_c25-533284.html
[4]: http://www.cisco.com/en/US/docs/general/warranty/English/LH2DEN__.html
[5]: http://www.cisco.com/en/US/docs/general/warranty/EnhLmtdLf_78-19324-01.html


