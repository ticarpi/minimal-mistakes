---
title: "JWT Tool Attack Methods"
categories:
tags:
  - "hacking"
  - "python"
  - "coding"
  - "security"
header:
  image: /assets/images/burp_jwt_pre.jpg
  teaser: /assets/images/500x300/png
---

So, having introduced my JWT auditing tool ([https://github.com/ticarpi/jwt_tool](https://github.com/ticarpi/jwt_tool)) in the [last post](https://www.ticarpi.com/introducing-jwt-tool/) I thought it would be good to provide a quick run-down of the various attack methods you should try as a pentester when faced with JWT auth on an engagement.  

## Token Generation
As with any web app pentest you should check the authentication process in place - as an injection here or bit of parameter tampering there could get you a privileged account without the need to mess with the tokens.  

## Token Transmission
Once the token is generated on the server it will be transferred to the client. This may be via a set-cookie Header, or in the response body.  
Either way, make sure that this token is sent securely, using TLS encryption.  

Bear in mind that in many cases a JWT is all that is needed to authenticate back to the server, so should be treated with all the same security considerations as a SESS_ID token.  

If this is being sent as a cookie then make sure it is set to ***Secure*** and ***HTTP-Only***.

## Token Vulnerabilities
A number of vulnerabilities can befall a JSON Web Token, so check these methodically.  

### Test for the *alg:None* vulnerability
This issue in unpatched JWT libraries will allow the algorithm in the JWT header to be changed from the current encryption scheme (*HS256*, *RS512* etc.) to using no signature.  

If vulnerable an attacker can simply tweak the header, and then change anything they wish to in the Claims section, and the server will accept it.  
(This is the attack seen in [Pentesterlab's JWT exercise](https://pentesterlab.com/exercises/jwt))


### Test for the RS/HS256 Public Key Mismatch vulnerability
This vulnerability in tokens signed with an RSA Private Key occurs when the server does not enforce the specified algorithm, and allows the token to specify this.  

If vulnerable the attacker can change the algorithm in the Header to HS256, and sign the token using the RSA Public Key (assuming they can locate it) via the HMAC-SHA256 algorithm. They can tweak anything in the Claims section before signing and the server will happily accept it as authentic.  
(This is the attack seen in [Pentesterlab's JWT_II exercise](https://pentesterlab.com/exercises/jwt_ii))

### Test for weak passwords
Developers may forget to change the default password in a bolt-on encryption module, or may set an easy password for testing purposes, and forget to change it when going live.  
*Or*... they may just have rubbish passwords like the rest of the world.  

Test the token for common weak passwords by specifying a wordlist/dictionary.  
Some good lists include the `RockYou` dumped password list, or any of the lists built into cracking programs like `John the Ripper`, or `SQLMap`.  

If you're using Kali Linux then you'll find a bunch of these in the `/usr/share/wordlists` directory.

## Forging and Replaying Tokens

Once you've found a weakness you can forge a new token...

![Installing JWT_Tool.py and forging a vulnerable token](/assets/images/jwt_privesc_alg_none.gif)
  
`JWT_Tool.py` has the tools to find these weaknesses, and then you can select the ***Forging*** option and tweak the Claims section of the token to your liking before signing:
* with the key
* specifying the *None* 'algorithm'
* or using the Public Key to sign the vulnerable RSA-signed token

To replay your forged token just fire up `Burp Suite`, or your favourite *intercepting proxy* to edit the data in transit, or set the new cookie in your browser via the console, or a cookie manager plugin.  

Hope you enjoy smashing those tokens!  
*ticarpi*


