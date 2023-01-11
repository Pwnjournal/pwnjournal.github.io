
---
layout: post
title:  Hack the Box rev Golfer Part 1
image: /assets/img/HTB-Logo.png
date:   2023-01-10 21:11:01
tags:
- rev
description: Basic rev challenge
categories:
- Hack The Box
---

Description

A friend gave you an odd executable file, in fact it is very tiny for a simple ELF, what secret can this file hide?



This file is very easy if you have a decompiler such as Ghidra or IDA, else you might be stuck.

First of all, let's look at the start of the executable:

![](/assets/img/2023-01-10-22-18-44.png)

We can see that 2 syscalls exist. The first is for EXIT. This will not help us, and it is run by default, and the other one is for write.

By looking at the executable we see other code that does not compose any function above:

![](/assets/img/2023-01-10-22-21-35.png)

We can also see a zone with strings that could make up our flag:

![](/assets/img/2023-01-10-22-20-45.png)

Analysing the code above reveals that it will load a string offset and then call the print function outlined above.

So redirecting the flow of the program to this part of it yields the flag:

HTB{y0U_4R3_a_g0lf3r}