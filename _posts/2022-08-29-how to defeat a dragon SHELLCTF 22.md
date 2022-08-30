---
layout: post
title:  how to defeat a dragon SHELLCTF 22
image: /assets/img/SHELL CTF 2022.png
date:   2022-08-29 21:52:19
tags:
- rev
description: Getting hex from the program and converting it twice to string easy rev

categories:
- SHELL CTF 22
---

Getting hex from the program and converting it twice to string easy rev

![](/assets/img/2022-08-13-09-59-47.png)


Dragonairre,the dragon with the hexadecimal head has attacked the village to take revenge on his last defeat,we need to get the ultimate weapon. Flag Format : SHELLCTF{}.


We see that the execution flow is very simple:

![](/assets/img/2022-08-13-10-01-46.png)

The input is compared with 69420 and if we input that the flag is printed, the flag is also encoded in the hex code above:

Converting the hex from the flag we get the real flag:
![](/assets/img/2022-08-13-10-03-15.png)