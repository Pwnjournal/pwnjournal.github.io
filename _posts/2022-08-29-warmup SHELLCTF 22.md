---
layout: post
title:  warmup SHELLCTF 22
image: /assets/img/SHELL CTF 2022.png
date:   2022-08-29 21:52:19
tags:
- rev
description: Getting string from challenge and then doing bitwise operations in reverse easy rev

categories:
- SHELLCTF 22
---

Getting string from challenge and then doing bitwise operations in reverse easy rev

![](/assets/img/2022-08-13-10-07-50.png)

This challenge is straightforward as well, in order to get the flag you only need to do a bitwise operation:

![](/assets/img/2022-08-13-10-08-29.png)

```
a=[460,416,404,432,432,396,464,408,492,392,196,464,348,420,212,404,380,192,448,204,456,260,464,192,456,332,500]
for i in a:
    print(chr(int(i/4)),end='')
```
Gives us:  
`shellctf{b1tWi5e_0p3rAt0rS}`