---
layout: post
title: Network Automation with Python, Ansible, and Twitter Integration
tags: network automation, python, ansible, twitter
---

Last month I [wrote](/home/leveraging-cisco-nx-api-with-ansible-to-make-your-life-easier) about using the Cisco Nexus NX-API to extract stats from a Nexus switch while using Ansible.  For some reason, last night I finally went on to tackle how to integrate with the Twitter API and then integrated the two together.  Integrating with Twitter has always been top of mind, but just put it on the back burner.  Funny enough though, it was a pretty quick integration thanks to the great people at [Google](https://code.google.com/p/python-twitter/).

### What am I talking about?

[In the code I pushed last month](https://github.com/jedelman8/nxapi-intf-ansible), I created an Ansible playbook that pulls interface stats from a Nexus 9000 (or any other Nexus device supporting NX-API) and then creates a template report for those stats.  It was pretty vanilla, nothing fancy about it.

There have been integrations with other social platforms, but to be honest, the one that has been stuck in my brain is [Hubot](https://hubot.github.com/) that is used at GitHub.  Several months back I remember hearing about Hubot for the second or third time while listening to the [Cloudcast podcast with Mark Imbriaco](http://www.thecloudcast.net/2014/03/topic-1-youve-run-ops-for-some-pretty.html) (just before he went to Digital Ocean).  It is amazing to see what can be done with the right team, culture, and tools.

Anyway, last night I figured I would check out using the Twitter API and integrate with Python assuming that it would be complicated, require “hard core” integration, and lots of time. It was totally the opposite – thankfully I found the Google python-twitter library on my first Google search!  It took just a few minutes.

Following the integration, the same playbook from last month now tweets 3 times.  It tweets when the playbook starts, after data is pulled from the Nexus 9000, and right before it ends.  Having tweets mid-way through longer playbooks could be good too.  You can tweet to certain people (or secure handles) or include whatever hashtag you’d like for easy tracking on changes.  Right now, the user of this playbook should modify the key_id variable for each execution – Twitter doesn’t like it when you tweet the same message repeatedly :)

The [GitHub repo](https://github.com/jedelman8/nxapi-intf-ansible) has been updated with the new playbook that has “with_twitter” in the file name.  The repo also has the ‘tweet’ module that I created.  Note: I am only using the twitter API to send tweets right now, but the library has dozens of methods built-in for gaining status messages, mentions, timelines, etc., so it is in fact a pretty robust library.

### The Twitter Integration

The [steps](https://code.google.com/p/python-twitter/) here are straightforward (also [here](https://github.com/bear/python-twitter)), but you will have to create an [official “Twitter app” first](https://dev.twitter.com/) and then they’ll give you the proper tokens and IDs to use while writing your code.

If you look at the playbook I’m using, you’ll notice I’m passing ‘creds’ to the tweet module.  ‘creds’ is just a variable I defined in another file so you can’t see it.  ‘creds’ in my case is a string that contains the 4 variables needed to use the twitter API.

This will be the flow to follow to test:

```
import twitter
api = twitter.Api(consumer_key='consumer_key', consumer_secret='consumer_secret', access_token_key='access_token', access_token_secret='access_token_secret')

print api.VerifyCredentials()

status = api.PostUpdate(‘this message will post to twitter')
```

If you have any questions, feel free to reach out!

Thanks,
Jason

Twitter: @jedelman8