---
title: "Hacking Android - My First CVEs! (CVE-2016-2457, CVE-2016-3760)"
excerpt: "The story of my first successful security research - FIXED - and a bug bounty from Google."
categories:
tags:
  - "cve"
  - "android"
  - "hacking"
  - "mobile"
  - "bug-bounty"
header:
  image: /assets/images/wifiproxying.png
---

*This blog post is a central point for the reports and blogs about the Android Guest Mode vulnerabilities I reported to Google in February 2016 - CVE-2016-2457 and CVE-2016-3760  
To read more about the vulnerabilities and how they are exploited read [the blog on the e2e-assure website](https://www.e2e-assure.com/Hacking_Android_Guest_Mode).  
The story of how they came to be follows below...*

Well this was a bit of a WIN for me.  
Just a month into my new career in infosec I was messing around with Android and I suddenly have a thought: *I wonder...*

In my experience this is what I do best - thinking outside the box; imagining scenarios that break the pattern, that do something unexpected.


#####Testing some stuff
Ever since Guest Mode was announced in Android 5.0 (Lollipop) - back in late 2014 - I had been curious about how allowing someone physical access to a device could be a *good idea*.
It is generally considered in security circles that:  
>physical access == device pwned  

This is a good principle that certainly holds true for Windows machines, but has truth on most platforms/OSes.

With this is mind I had long intended to *get to know* the workings of Android, and to understand the disconnect in between the ***Owner Account*** and the ***Guest Account***.
Finally in the New Year of 2016 I got around to doing this.

I tested the UIDs for the accounts, file paths; attempted directory traversal, tried to exploit running services, play with *intents*, and a bunch of other interesting angles - all to no avail. This is a properly separated account, and everything has a disconnect, a divide to keep it secure.

Then one day I was preparing for some unrelated testing on my smartphone and started setting up an *intercepting proxy* (via the excellent [Fiddler](http://www.telerik.com/fiddler)) to capture the web traffic from an app. Suddenly I had a thought about lack of separation in account settings.
I had a good feeling about this, so I set up a few tests on my Nexus 4, switching between accounts, playing with settings - seeing what stuck.
All the main *dangerous* settings were inaccessible in Guest Mode, other settings were limited. However there were a number of the Quick Settings that were not blocked...

>Indeed, the very one I was most interested to test was accessible.

So, I tweaked the setting and checked that the traffic from Guest Mode was being sent through my remote device. It was - as expected.
Then came the real test - *would the settings persist in the Owner Account?*

I switched user, then opened the Advanced Wi-Fi settings... The proxy settings were still there!
I opened the browser to double-check. Sure enough, my traffic started popping up in Fiddler.
I tested again to be certain. Then I cleared the settings and tried once more - all successful.  

This was the kicker though - ***this is not just Privilege Escalation***.  
Once launched for the first time by the owner, Guest Mode persists on the Android device. It is accessible from the lock-screen.

>I had found a way for setting up invisible remote monitoring of all device traffic from a locked Android device!

####More fun?
After getting myself a little bit excited about the implications of this, I tested out some more settings - most were innocuous.  
However, I did managed to find one more that could do some damage - Bluetooth.

I found that authenticating a new Bluetooth device was possible in Guest Mode, and that it also persisted into the main user account.
You couldn't set up SmartLock on it, and you couldn't pre-authorise it to download content from the main account, however you could do a whole load of *naughty things* without the owner knowing that you had set it up.

>My main avenue of attack was HID attacks  

This attack involves sending a fast burst of programmed keystrokes from a virtual keyboard. As the owner may not realise my new Bluetooth device was connected (s)he wouldn't know what was going on, and would be unlikely to be quick enough to prevent the changing of settings, the download and installation of malicious APKs, or a bunch of other creative attack vectors.

In addition we could pre-authorise a Bluetooth headset to listen to calls, using this to intercept 2-Factor authorisations and the like.

#####Reporting and Bug Bounties
With two exploits in the bag I reported these via Google's Android [Security bug report tracker](https://code.google.com/p/android/issues/entry?template=Security%20bug%20report) and waited for a response.

Within a day or two I had confirmations of receipt; with further acknowledgements of eligibility for a bug bounty, as well as updates on bug fixing progress over the following weeks. The issues are now both fixed in Android, and third party vendors are pushing these fixes out to their devices *in their own sweet time...*

I was very pleased with the process and feedback, and was grateful to receive a payment for my time and efforts.
However, what really made my day/week/year was my permanent inclusion on the [Android Security Acknowledgements](http://source.android.com/security/overview/acknowledgements.html) page, and the CVEs registered for my reported vulns (CVE-2016-2457, CVE-2016-3760).

As an aside - I had the chance to meet the Android and Google security/VRP teams at ***BountyCraft 2016*** a few weeks ago, and they are great folks. I really hope that my ongoing research will give me more excuses to pester them about Android, and allow me to keep working to help keep our mobile devices secure.