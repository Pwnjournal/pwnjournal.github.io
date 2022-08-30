---
layout: post
title:  Microsoft corctf2022
image: /assets/img/CORCTF 2022.PNG
date:   2022-08-29 21:52:19
tags:
- rev
description: linux executable that has half a section in 32 bit and one in 16 bit

categories:
- rev
---

linux executable that has half a section in 32 bit and one in 16 bit

We see that the executable has a string location and that before comparing with our input string (read through int 0x80) the file uses rol with 0xd:  
![](/assets/img/2022-08-12-08-53-26.png)

In order to do the same but backwards we use cyberchef  
![](/assets/img/2022-08-12-08-54-00.png)

We note the comparison chars but change our input chars:  
for the first char it is:  
set $al=0x6c  

The next chars are compared we need to have 0x12 characters in total:  

![](/assets/img/2022-08-12-08-59-51.png)

Checking the code it is not good so we have to disassemble the whole binary and see other parts of code  

After looking through the message:  

![](/assets/img/2022-08-12-09-26-02.png)

I found a section of code that didn't compile right. I edited the segment as to be 16 bit ( edit->segments->edit segment). And then by pressing C in ida i made the section code:  

![](/assets/img/2022-08-12-09-27-26.png)

We see that it xors the string with 0xd:  

3nd,3Xt1ngu15h!!1}  

So combining them:  

corctf{3mbr4c3,3xt3nd,3Xt1ngu15h!!1}  