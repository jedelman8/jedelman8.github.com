---
layout: post
title: Application Routes with onePK and the Need for Better Libraries 
tags: onepk
---

It’s been some time since I wrote about Cisco’s onePK.  In this post, I’ll look at some of the good and the bad of onePK and also show an example of how to add a route to a device running onePK to help make a few points along the way.

### The Bad

I’ve never heard anyone speak positively about onePK and I’m not sure I 100% agree, but I’ll save the positivity for the next section.  onePK is a thick Software Development Kit (SDK).  If you are a network engineer looking to learn to program from the ground up, it may NOT be the BEST place to start.  That said, if you are looking to learn about object oriented programming, listener APIs, etc., and can spare some time, it’s a great place to start.  If you’re already a developer, it probably won’t be much different compared to learning any other SDK.

Another thing to be weary of is that onePK was not intended to be a configuration API.  I voiced my [opinion](https://communities.cisco.com/thread/41789) on this already and I do think things are headed in the right direction, but it always helps knowing the history.  Initially, it was about dynamic configurations.  This means that if the onePK app disconnected from a device, all configs that the app provisioned would be de-commissioned.  Not cool.

Lastly, if you are going to use onePK don’t forget to cross check SDK support with platform support.  There is not 100% uniformity, but hey, you may not need it anyway given what you are trying to accomplish.

### Adding a Route the onePK Way

You can easily see the complexity here.  Look at all of the modules required for what “should” be a simple task.

![onepk-1](/img/one1.png)

The following picture shows the process to create and send the route to a device.  The screen shot shows it in a function that I created to simplify the process and you’ll see that more in the next section, but you get the idea.  15 commands to add a route seems like it’s a few too many.  What do you think?

![onepk-2](/img/one2.png)

### The Good

Cisco DevNet offers an all-in-one onePK virtual machine.  You can download a virtual machine that has a 3 node virtual router topology that has Java/C/Python onePK libraries pre-installed.  Each router, of course, supports onePK.  The VM also has all of the onePK documentation bookmarked in a FireFox browser.  This is real.  It exists today.  It is one of the best learning VMs I’ve seen for network programming.  So, to those who want to wait for VIRL to start programming, keep waiting.  I do think it’s the better option, but at the end of the day, if you are just looking to understand the basics of working with an API, it does not matter and this is a real solid choice.  For this reason, I think onePK is just fine.  While challenging, you’ll learn quite a bit and then be able to recognize when you work with more consumable APIs like Cisco NX-API or Arista eAPI.

### Adding a route my way

After abstracting out the creation and deployment of a route into its own function, all you need to do is create a single variable (Python dictionary) and store the data about the route you want to add.  Then call the addRoute() function by sending it two arguments: (1) the device object which should be of type NetworkElement and (2) the route in the form of a Python dictionary.


![onepk-3](/img/one3.png)

This isn’t showing the details of the Device module, but that’s something I also created to simplify setting up devices in onePK.

### Consumable Libraries

As you can see, we need more “widgets” like addRoute() and Device().  These are just two quick examples, but if consumable libraries were out there, it would increase the rate of adoption and learning.  The best example of a comprehensive consumable library is from Juniper.  The [Juniper PyEZ](https://techwiki.juniper.net/Automation_Scripting/010_Getting_Started_and_Reference/Junos_PyEZ) library abstracts away having to know anything about the Juniper underlying Netconf/XML interface. 

There is no reason libraries can’t be created to ease consumption of the “off the shelf” APIs and libraries from every vendor.  In the meantime, we can all keep creating small modules (“widgets”) like this to make our lives easier. 

FYI to the vendors: This is not just a onePK thing.  We need more consumable libraries all around.  As new vendors emerge and incumbents come out with APIs, please think through who your target audience is and how you will help them consume your API.

Thanks,
Jason

Twitter: @jedelman8