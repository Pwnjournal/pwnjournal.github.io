---
layout: post
title:  secret document SHELLCTF 22
image: /assets/img/SHELL CTF 2022.png
date:   2022-08-29 21:52:19
tags:
-forensics
description: Document xoring with given key, easy for
categories:
- SHELLCTF 22
---

Document xoring with given key, easy for

![](/assets/img/2022-08-13-10-15-24.png)

This is a very easy challenge

We take the document xor it with shell and then we get the following picture:

![](/assets/img/2022-08-13-10-19-11.png)

We then see that it is a png by magic number:  

open and get flag:


![](/assets/img/2022-08-13-10-19-40.png)