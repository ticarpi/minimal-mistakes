---
title: "Thought Experiment #1 - Basic Exfil"
excerpt: "Experimental! A quick blog on thinking a way out of a problem."
categories:
tags:
  - "thought-experiment"
header:
  image: /assets/images/usr_bin2.png
  teaser: /assets/images/500x300.png
---

I hope you're ready for this!
This post is to introduce a theme I plan to use occasionally on this blog - *Thought Experiments*.

In short: These are the kind of thoughts that pop into my head while I'm doing something at work, and I wonder whether something is possible if you tweak one of the normal features. Or perhaps, if I am looking at something that someone has already done, I wonder if I could do it more efficiently, or in a new way.

This particular thoughtexp occurred to me when needing to get some data off a remote box. I used an approved encrypted method, but it did make me wonder...

> **ThoughtExperiment#1:**  
> If you were on a linux machine with no tools and limited access - how would you exfil a file by a text-only protocol while limiting the chances that someone capturing traffic would be able to easily read it?

I suppose the scenario could be blackhat - a hacker with a limited-level exploit; or whitehat - needing to get a sensitive file off a friendly box.

*Disclaimer: If you're copying any client files ALWAYS do so securely. This hacky thoughtexp is just a game for keeping the mind supple.*

## The thinking:  
OK, so what do we have?  
[+] Basic tools. Commandline stuff  
[+] Ingenuity (it's pep talk time)

What don't we have?  
[+] Time (this is a challenge dagnabbit)  
[+] Anything for basic encryption  
[+] Interfaces for transferring files - this will be text-only  

**What can we use then?**  
OK, well nobody installed *openssl*, and with nothing else encryptish in sight we can't get this data password-protected. Great...
If this is text-only we can't send it in a block file format. (In my mind I'm going to be streaming it out with *netcat* or something similar.)

***So, let's obfuscate it into a total (yet reversible) mess!***  
Base64 is somewhat overused, but very useful, and installed by default on all *nix systems I know.
But base64-encoding a string really isn't going to cut it. Those strings are easy to recognise and simple (or automatic) to decrypt.
Hmm, so how to make the base64 string harder to recognise or read. Those hanging equals signs are a bit of a giveaway - so let's strip those off. When we receive the file we can add any in to pad the string to a multiple of 4.

Still, if the string is already a multiple of 4 there are no equals signs to drop, and the string can be read easily.
Right, let's reverse it then!

The *rev* tool will reverse all text on a line:

```bash   
$ echo "Hello World." | rev
.dlroW olleH
```

That's grand, that is.
Oh, hang on - multiple-line text files still are in line-order:

```bash
$ cat multi.txt | rev
1# enil si sihT
2# enil si sihT
```
OK, well did you know about *tac*?
Yep, that's *cat* backwards...

```bash
$ tac multi.txt | rev
2# enil si sihT
1# enil si sihT
```
Ha. #winning

OK, so that's going to be pretty obfuscated.
I'm going to reverse the text, then reverse the base64 string too (*and why not??*).
Let's grab some *sed* and then see how that looks as a one-liner:

```bash
$ tac exfilme.txt | rev | base64 -w 0 | rev | sed 's/=//g'
oASlJXZgk2cgEGIzRnclFWbg8mZgQWY0FmLgkEdgoyYvVHbkpCIiVGIj9mbmlGZl5GdpFGbuoQS0BSaz52J05CIUhWazBSazBia1NHdgEGI0V2c05iCIVmclBSazBSYuBCcpNGd1JXZ6oAZo42XuliY
```

Right, that's pretty cool.
Recognising the string as possible base64 isn't going to do you any good. So even if you pad it out the string is just junk now:

```bash
$ echo oASlJXZgk2cgEGIzRnclFWbg8mZgQWY0FmLgkEdgoyYvVHbkpCIiVGIj9mbmlGZl5GdpFGbuoQS0BSaz52J05CIUhWazBSazBia1NHdgEGI0V2c05iCIVmclBSazBSYuBCcpNGd1JXZ6oAZo42XuliY= | base64 -d
��%v�g b3Fw%f��fAf4b�G�&/Tv�--Tb%�f�fe�g&��bt��f&�&�4wb4Wg4� �V&�&.)4gu%vz�h�e
```

Fancy one more safety precaution?
Let's just hide the string a bit more by putting some padding at the beginning. Then you have to strip that off before reversing.
(Hacked-together way of grabbing only ASCII values from the random-char generator FTW!)

So, the final code goes like this:
```bash
$ cat /dev/urandom | tr -dc 'a-zA-Z0-9' | head -c 4 && echo $(tac exfilme.txt | rev | base64 -w 0 | rev | sed 's/=//g')
```
So when you receive the string you can strip off the known quantity from the *head* portion of the command above - 4 here, but could be anything. Then you can reverse the whole thing and you're all good to go.

[end braindump]
