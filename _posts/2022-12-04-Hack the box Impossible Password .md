---
layout: post
title:  Hack the Box rev Impossible Password
image: /assets/img/HTB-Logo.png
date:   2023-01-04 18:11:01
tags:
- rev
description: Basic rev challenge
categories:
- Hack The Box
---

## Impossible Password

Easy  

CHALLENGE DESCRIPTION

Impossible Password

The main function looks like this:

![](/assets/img/2023-01-04-21-56-19.png)


Seeing such a string and comparison chain leads us to believe the flag is the first string decrypted/decoded.

Following the chain we can see that there is the last function which decrypts the flag:

![](/assets/img/2023-01-04-21-57-44.png)

We can see that the flag is xored with 9 to make it printable.

We do the same operation:

![](/assets/img/2023-01-04-21-58-17.png)

alternatively in GDB we execute the file and redirect the flow to the function or bypass the two strcmp:


![](/assets/img/2023-01-04-21-59-48.png)

and afterwards:

![](/assets/img/2023-01-04-22-00-24.png)

and after the flow will just continue to printing the flag.

One part of the password is random generated so the only way is bypassing the compare/patching the binary.