---
title: "Chromebooks FTW!"
excerpt: "Coding, hacking and cmdline-fu on a Chromebook"
categories:
tags:
  - "coding"
  - "devices"
  - "chrome"
  - "python"
  - "security" 
header:
  image: /assets/images/coding_on_chrome.png
---

I've been playing with Chromebooks for a while now.
I got my first one (an Acer C720) a few years ago, and have done a load of stuff with it - using it for a while, then leaving it for a bit, then picking it up again.

All in all it is fair to say that it is a divisive sort of machine. The naysayers says it's an underpowered half-computer that only works online. While the other camp (the *yaysayers*??) says they do everything that most people need.

You want to know the truth? I'm not *normal*.
I don't really fit into the simple categories of who may or may not deal well with a Chromebook.
So I just had to try them.

And I love it. I really do. And for a bunch of reasons.

#####Firstly, I use mine for malware analysis:  
One of the great things about Chromebooks is that the operating system is seriously secure.

The OS is updated automatically; it checks itself for corruption/infection every time it boots; it encrypts the user partitions; and it allows anonymous logon via Guest mode.

So, by using Guest mode I use my Chromebook for visiting potentially infected websites. I can safely view the sites, analyse them through Chrome's built-in Dev Tools, download and debug any scripts on the sites, and check numerous other aspects of site security.

And once I've done all that I just reboot into my normal account and all is well. Whoop


#####Secondly, I use it for coding  
I have found a marvellous use for my latest Chromebook - an Acer R11 (a convertible, flippy, touchscreen thing).
>I had to get another one - my wife keeps on borrowing my C720

You see, some epic fellows have found that by switching a Chromebook to Developer mode you can install a chroot (fancy name for running multiple Linux-based operating systems at the same time.
So... I have installed a command-line version of Ubuntu via [crouton](https://github.com/dnschneid/crouton), which I run from the Chrome terminal (crosh), while editing code on useful Chrome apps (like [the Text app](https://chrome.google.com/webstore/detail/text/mmfbcljfglbokpmkimbfghdkjmjhdgbg)) and using git for version control and sharing.

#####Thirdly, Android apps are coming!!  
Yep, Android apps (and [the whole Google Play Store](https://chrome.googleblog.com/2016/05/the-google-play-store-coming-to.html)) are coming to Chromebooks by the end of this year - or thereabouts.
My R11 should be getting the update any day now, and the reports for those already testing the update are largely very favourable.

Android apps on Chromebooks will be a ***total game-changer***, with Chromebooks being cheaper than good quality tablets, while being better for typing and multi-tasking.
They are more secure than Windows and Mac OS, faster to boot, slick for website stuffs, and can do most things offline nowadays - which Android will improve upon.

I will also be expanding my Android security testing my analysing the Play Store integration as soon as my R11 gets the go-ahead.
I will also be using it as another platform for testing Android apps on for other security purposes.


*Lots of fun to look forward to!!*