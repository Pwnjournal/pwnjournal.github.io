---
layout: post
title:  Rebasing an image
image: /assets/img/IDA tricks.jpg
date:   2022-08-29 21:52:19
tags:
-rev 
description: When we load an image in GDB (pwndbg) and Ida the address where the executable is placed in memory can be different. To rebase the Program in IDA select Edit->Segments->Rebase program and in gdb if you are using gef you can use  "pie break *0xoffset" and that's it and if you are using pwndbg use the piebase command to find the address of the binary and then in ida set the base binary image address to that one.

categories:
-rev
---

When we load an image in GDB (pwndbg) and Ida the address where the executable is placed in memory can be different. To rebase the Program in IDA select Edit->Segments->Rebase program and in gdb if you are using gef you can use  "pie break *0xoffset" and that's it and if you are using pwndbg use the piebase command to find the address of the binary and then in ida set the base binary image address to that one.

Initial Program main function:
![](/assets/img/2022-05-14-09-18-04.png)
Main function in pwndbg:
![](/assets/img/2022-05-14-09-18-48.png)
Finding out image base:
![](/assets/img/2022-05-14-09-23-54.png)
Rebasing in IDA:
![](/assets/img/2022-05-14-09-20-20.png)
![](/assets/img/2022-05-14-09-20-48.png)
Result:
![](/assets/img/2022-05-14-09-22-49.png)