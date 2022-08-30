---
layout: post
title:  Nuts and Bolts Reverse
image: /assets/img/HTB CA 2022.jpg
date:   2022-08-29 21:52:19
tags:
- rev
description: Nuts and Bolts
categories:
- rev
---

Nuts and Bolts
We've broken into Golden Fang's main weapons assembly facility! If we want to steal them for ourselves, we'll need to crack their top-secret bio-encoded locking scheme. Can you disassemble the weapons in time?
![](/assets/img/2022-05-19-14-35-59.png)
Looking at the code this challenge is very easy, because it's just a rust script.
![](/assets/img/2022-05-19-17-26-36.png)
The junk you see before any real information is the product of the bincode:serialize. We can decrypt the payload with a python script.

![](/assets/img/2022-05-19-17-37-18.png)
By looking at the binary included in the challenge we see that the xor operation is done using 0xD byte.
We reverse the operations and get the flag
![](/assets/img/2022-05-19-17-40-02.png)