---
title: "Picking 'unpickable' locks - security vs obscurity"
excerpt: "A mini-tale about lock-picking, assumptions, and an infosec metaphor"
categories:
tags:
  - "security"
  - "locks"
  - "apps"
header:
  image: /assets/images/discdetainer.jpg
  teaser: /assets/images/500x300.png
---

This morning a parcel arrived: a padlock I'd ordered from Amazon to practice lockpicking with.

    For those of you who might be concerned I'm turning to a life of crime - lockpicking is a time-honoured hobby practiced ethically for centuries by tinkerers and polymaths. It's a type of three-dimensional puzzle and a great way to improve spatial-awareness, patience, and dexterity - as well as being lots of fun.

As I unpackaged the lock I marvelled at its solid un-cuttable construction, shim-proof ball-bearing mechanism, and hefty weight. Then I took it to my workroom, set a timer and started to pick it.
*2 minutes and 32 seconds later* I felt the swing of the tension tool turning the zero-disc and the clank of the large shackle dropping open. Success!

But allow me to let you into a secret, good readers. This wasn't any old lock. Indeed, no. This was ***an unpickable lock***!

Or so I had been led to believe by customer J.A. Stewart's 5-star review of this padlock on Amazon...
>*"The keys are of a high security design and I would imagine that this lock cannot be picked easily if at all."*

lol.

![unpickable disc detainer](/content/images/2016/06/unpickable.jpg)

Of course I mean no disrespect to this reviewer, and I'm glad he took the time to write a detailed review for this item and help others who are considering future lock purchases. However his assumption that an unusual lock style (a [disc detainer](http://www.lockwiki.com/index.php/Disc_detainer) in case you're interested) correlates with good security is very common.

This illustrates the point I wish to make quite effectively.
>**Obscurity is not the same thing as security.**

As I see on a daily basis while monitoring client interactions with the internet, emails, and mobile apps - people assume that something being complicated or 'behind-the-scenes' is the same as something being secure.

Case in point: mobile apps often ask users for details - sign into this, type in your personal details here, pay for that.
These interactions happen, the data is sent somewhere, and an answer provided - or payment accepted.
*But how are these communications sent?*

More and more on the internet, people are understanding the need for the ***https*** bit in the web address when entering banking details, or that *padlock symbol* next to the web address.
People are coming to realise (*finally!*) that we need a secure connection before sending personal details or passwords.
But on mobile apps we don't have an address bar.
>we have to make the ***assumption*** that this standard minimum level of secure transaction is what is happening.

As I've delved deeper into mobile app security testing I have seen time and time again that app developers **don't** prioritise security at all.

* I have seen highly personal data being sent from apps to insecure webpages where it can be looked up by anyone (even though the app *promised* a high level of security)
* I've seen ad traffic in apps redirecting people to download malware without their knowledge
* I've seen apps built *so badly* that someone can click a couple of buttons to ***entirely skip the login process*** and enter the account in the app without a password!

This month Apple announced their commitment to improving security of iOS apps by (soon) forcing developers [to use https](http://www.techrepublic.com/article/wwdc-2016-apple-to-require-https-encryption-on-all-ios-apps-by-2017/) communications in their apps.
While this only covers one small aspect of app security as a whole it *is* an important step - and one I hope that Android follows suit on ASAP.

So a quick reminder:

* watch out for reports of insecure apps
* don't install apps from 'alternative' app stores
* and make sure you're happy with the reviews and permissions before installing **anything** on your mobile devices.

Stay safe out there!
