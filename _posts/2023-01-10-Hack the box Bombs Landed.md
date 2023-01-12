
---
layout: post
title:  Hack the Box rev  Bombs Landed
image: /assets/img/HTB-Logo.png
date:   2023-01-12 9:01:01
tags:
- rev
description: Basic rev challenge
categories:
- Hack The Box
---

Description

Can you find the password?
Enter the password as flag in the following form: HTB{passwordhere}



This executable has some annoying features such as anti-debug (easy to bypass) and also a custom strcmp function that is more annoying.

Looking at the binary start sequence we can see that there are 2 functions that are run before main:
![](2023-01-12-09-20-24.png)

One of them is a very simple anti-debug mechanism that alters the course of the program:
![](2023-01-12-09-21-06.png)

You can patch the exe or use debug to alter the funcion execution.

Afterwards you need to be careful how many arguments you feed the program. The check is made at:

![](2023-01-12-09-27-35.png)

4 arguments is enough (`h h h h`)  

After you pass these checks another function will be created that will contain the flag. The flag will be xored with the key 0xA and then compared all in the strncmp function.
Calls the new main:



![](2023-01-12-09-38-12.png)


The new main with encrypted strings:

![](2023-01-12-09-38-55.png)

Encrypted Flag:

![](2023-01-12-09-41-10.png)

key in fake strcmp:

![](2023-01-12-09-41-46.png)


Flag:


![](2023-01-12-09-43-08.png)




