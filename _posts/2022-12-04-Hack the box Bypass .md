---
layout: post
title:  Hack the Box rev Bypass
image: /assets/img/HTB-Logo.png
date:   2023-01-05 12:11:01
tags:
- rev
description: Basic rev challenge
categories:
- Hack The Box
---

## Bypass

Easy  

CHALLENGE DESCRIPTION


The Client is in full control. Bypass the authentication and read the key to get the Flag.  

The executable is made in .NET as exposed by PEStudio:  

![](/assets/img/2023-01-05-11-29-25.png)

Looking around in the disassembler we see that there are multiple strings that are being built.  

After putting the breakpoint on one of the returns we see that the charmap contains:  

`i c e   h e r e   i s   t h e   F l a g : H T B {`

![](/assets/img/2023-01-05-11-41-25.png)

We analyse the function and it's usages and find out where the string 5.5 is used.  

![](/assets/img/2023-01-05-11-27-53.png)

Afterwards we get the execution flow of the program to go there by changing the flags to true whenever a comparison is made.

The second password is ThisIsAReallyReallySecureKeyButYouCanReadItFromSourceSoItSucks.  
You can get it from watching the string reading.

The flag given is: 		

string.Concat returned	"Nice here is the Flag:HTB{SuP3rC00lFL4g}"	string



