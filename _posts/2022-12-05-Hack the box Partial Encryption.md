---
layout: post
title:  Hack the Box rev Partial Encryption
image: /assets/img/HTB-Logo.png
date:   2023-01-07 21:11:01
tags:
- rev
description: Basic rev challenge
categories:
- Hack The Box
---

Description

Static-Analysis on this program didn't reveal much. There must be a better way to approach this...



The challenge uses a function to set execute permissions to different parts of the code as well as .

The execution chain is happening like this:

![](/assets/img/2023-01-07-21-28-59.png)

The function that prepares functions takes 2 arguments

rcx which gets an address and rdx which gets a size

There is then a loop which calls a decryption function for blocks of code :


![](/assets/img/2023-01-07-21-38-17.png)

Afterwards execute permissions are set and the function is called.


The important comparisons are

the flag should be >0x16
it should have HTB{

and then the following decrypted functions:

1.
![](/assets/img/2023-01-07-21-26-45.png)
2.
![](/assets/img/2023-01-07-21-42-03.png)
3.
![](/assets/img/2023-01-07-21-45-21.png)

Give us the flag:

HTB{W3iRd_RUnT1m3_DEC}