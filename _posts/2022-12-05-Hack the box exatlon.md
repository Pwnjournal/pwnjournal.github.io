---
layout: post
title:  Hack the Box rev Exatlon
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


Can you find the password? 




The executable is packed with UPX. This can be found by running the strings command like so:

strings file | grep UPX


![](/assets/img/2023-01-05-16-18-45.png)



Afterwards to unpack it just run :

```
upx -d exatlon_v1 
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2020
UPX 3.96        Markus Oberhumer, Laszlo Molnar & John Reiser   Jan 23rd 2020

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
   2202568 <-    709524   32.21%   linux/amd64   exatlon_v1
```

Running the debugger on the executable and inputting any chars we notice that our chars are changed by the exatlon function. They become numbers. 

The part of the program that matters is:

![](/assets/img/2023-01-05-16-43-36.png)

The 0x61 ('a') gets changed to 0x610. And then converted to string (1552). Because 1552 is the decimal for 0x610 .


We also see that at the end, in the main function it compares the result with `1152 1344 1056 1968 1728 816 1648 784 1584 816 1728 1520 1840 1664 784 1632 1856 1520 1728 816 1632 1856 1520 784 1760 1840 1824 816 1584 1856 784 1776 1760 528 528 2000`.

![](/assets/img/2023-01-05-16-54-31.png)

The solving problem becomes reversing the operation on the given numbers:

a="1152 1344 1056 1968 1728 816 1648 784 1584 816 1728 1520 1840 1664 784 1632 1856 1520 1728 816 1632 1856 1520 784 1760 1840 1824 816 1584 1856 784 1776 1760 528 528 2000"
''.join([chr(int(int(i)/16)) for i in a.split(" ")])


which gives us the flag:
'HTB{l3g1c3l_sh1ft_l3ft_1nsr3ct1on!!}'



