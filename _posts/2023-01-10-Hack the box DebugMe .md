---
layout: post
title:  Hack the Box rev DebugMe 
image: /assets/img/HTB-Logo.png
date:   2023-01-11 19:11:01
tags:
- rev
description: Basic rev challenge
categories:
- Hack The Box
---

Description

A develper is experiementing with different ways to protect their software. They have sent in a windows binary that is suposed to super secureand really hard to debug. Debug and see if you can find the flag.

The executable is an anti-debug check with the flag being put on the stack at the end.

The first debug check is done by getting the PEB located at fs:30h and getting the ISdebugger bit from it. Normal would be 0 and I had 0x1


![](/assets/img/2023-01-12-19-29-58.png)



The second check is done by PEB.NtGlobalFlag (offset 0x68, normal 0x0), i had 0x70




![](/assets/img/2023-01-12-19-30-14.png)

Further down the executable it will make and call main which will do the same checks.

The last debug check is made by timing with 2 rdtsc instructions .

![](/assets/img/2023-01-12-20-08-55.png)

Afterwards the main function is built by xoring some memory. The main will contain the same checks seen here and afterwards the flag will be built on the stack

![](/assets/img/2023-01-12-19-30-42.png)



The flag is not printed so you have to grab them at return.


![](/assets/img/2023-01-12-19-45-18.png)



![](/assets/img/2023-01-12-19-44-43.png)




HTB{Tr0lling_Ant1_D3buGGeR_trickz_R_fun!}