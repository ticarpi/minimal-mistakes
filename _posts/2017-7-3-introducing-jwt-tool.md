---
title: "Introducing JWT Tool"
categories:
tags:
  - "hacking"
  - "python"
  - "coding"
  - "security"
header:
  image: /assets/images/jwt_tool.jpg
---

>This post is to introduce you to a python project I've just published, having found a void and deciding to be the one to fill it...

The **JWT Toolkit** ([https://github.com/ticarpi/jwt_tool](https://github.com/ticarpi/jwt_tool)) is an auditing tool for ***JSON Web Tokens*** - which serve an **Authentication** and **Authorisation** function on countless websites across the interwebs.  

When properly implemented JWTs are a good, strong option to provide this function.  
However they do have some inherent weaknesses, which if misunderstood can lead to a false sense of security.  

This toolkit is intended to be used by **pentesters** looking to easily test the tokens they come across in an engagement; and also by **developers** looking to better understand, and more effectively secure their JWT-protected web applications.


## A brief JWT Primer
JWTs are comprised of three parts:
* Header
* Content/Claims
* Signature

The first two parts are JSON lists, which are Base64-encoded and squidged together with a dot in between.
To round it all out these parts are signed server-side using a secret or key - which is added with a dot onto the end.  

The result is a token which can be sent as a cookie, as an HTTP Authentication Header, or stored in Session- or Local- storage and passed as a GET/POST parameter.  

> eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJsb2dpbiI6InRpY2FycGkifQ.aqNCvShlNT9jBFTPBpHDbt2gBB1MyHiisSDdp8SQvgw

Someone new to these tokens may assume they offered some privacy, however the decoding of the first two parts is just a simple Base64 decode:

> {"typ":"JWT","alg":"HS256"}.{"login":"ticarpi"}.[signature]

So, there's your first tip - don't store anything private in the token.  
*Yes, people have been known to store plaintext passwords in there(!)*  

As you will find by reading Tim McLean's [great research on JWT weaknesses](https://auth0.com/blog/critical-vulnerabilities-in-json-web-token-libraries/), there are a number of weaknesses found across the various JWT libraries in use on production webservers across the internet.  

These weaknesses revolve mainly around the failure to enforce the algorithm in use when checking the signature.  
This means that in some cases the user can reforge the token to specify either to *ignore the signature altogether*, or to force the use of the server's ***Public Key*** as the secret in use in the HMAC-SHA algorithms!

Aside from these, there are the *normal* weaknesses that developers introduce by themselves: such as insecure passwords, and poorly-constructed token generation code.

All together that gives us a chance to play at a high-stakes table.  Which is where this tool comes in.  

## The Toolkit
I wrote this toolkit to enable pentesters to easily download this one tool and run it with the JWT as the only argument.  

It provides a helpful menu for:
* forging new tokens to test the various vulnerabilities
* editing the content of the *claims* section
* testing possible keys - either individually, or via a wordlist.

I chose to include a Dictionary Attack mode, as it was nice to keep everything in one place - even though the latest versions of `John The Ripper` do have a JWT mode.  
Hopefully you'll find the speed acceptable - in my tests on an Intel i5 laptop I can test over 1 Million passwords/second. *YMMV*  
I consider this reasonable.  


## Bit of Back-Story
*Please excuse this little bit of indulgence...*  
> I have a bit of a *relationship* with JWTs.  

I had never met them before my ***first*** web app pentest engagement - and was intrigued when I spotted them in use and started reading up on them.  
The tantalising text in the token which was restricting my privileges on the system was too much to ignore, and **I knew I had to find a way** to use them for privesc.

Half an hour later I suddenly hit upon a way to force an *overly-chatty* error message in the login function and there lay the key used to sign the token - right before my eyes.  

A guess at the syntax, and a bunch of Base64-juggling in the Terminal, and I was ready to sign the forged token with the key. I used [JWT.io](http://jwt.io) to create this new token, and injected it into my web traffic.  

{"Privileges","**ReadOnly**"} became {"Privileges","**Administrator**"}, and I had full access to all the data, settings and access control.  

All in all it was a fine first meeting, but I still had a strong desire to test the other possible weaknesses in JWTs on any other system I came across them.  
This was doable through manual processes, but when you start tweaking the available JWT libraries to accept malformed input it became quite clear that a proper JWT hacking and forging toolkit was what was called for.  

*Call it a vanity project if you will*, but I wanted something to practice my limited Python skills on, and so I decided to shun any of the existing (excellent) JWT libraries, and build one myself from scratch.

So here it is - native Python code, no libraries to download, and a full-featured JWT attack platform.  

`JWT_Tool.py` incorporates all the known vulnerabilities and weaknesses you are likely to meet in an engagement, and should allow you to forge and reforge tokens on a whim.  

I hope you enjoy it!  
*ticarpi*
