---
layout: post
title:  UNIBUC_RE_lab_5_task_4
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:
- pwn
description: This task you are provided with a seed and you need to give the random inputs, you are not allowed to input any uppercase chars so you need to relay on stack superposition and the program not clearing it before reading your other input
categories:
- UNIBUC RE
---

This task you are provided with a seed and you need to give the random inputs, you are not allowed to input any uppercase chars so you need to relay on stack superposition and the program not clearing it before reading your other input


![](/assets/img/2022-09-01-23-33-39.png)

The magic number is the rand seed

Making the password:

![](/assets/img/2022-09-01-23-34-01.png)


Check for upper case:

![](/assets/img/2022-09-01-23-35-17.png)

Comparison:

![](/assets/img/2022-09-01-23-35-39.png)

Buffer alignment:

![](/assets/img/2022-09-01-23-37-07.png)

![](/assets/img/2022-09-01-23-38-21.png)

So we can just only read 2  characters and hope they aren't uppercase.


Debug script:
```
from pwn import *

from ctypes import *
libc = CDLL("libc.so.6")


io = process("./riddle")


io.recvuntil("Today's magic number is ")
number=io.recvline()
libc.srand(int(number.decode(),16))
letters=[]
choices="ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/ "
for i in range(2):
    letters.append( choices[libc.rand()%0x41])
gdb.attach(io,'break *0x4014C5')

io.send(''.join(letters))
print(''.join(letters))
text=io.recv(timeout=1)
if b'serious' in text:
    io.close()
else:
    io.interactive()


```


Better optimised variant such that it tries all 10 chances.

```
from pwn import *

from ctypes import *
libc = CDLL("libc.so.6")


io = process("./riddle")


io.recvuntil("Today's magic number is ")
number=io.recvline()
libc.srand(int(number.decode(),16))

choices="ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/ "
for k in range(10):
    letters=[]
    for i in range(2):
        letters.append( choices[libc.rand() %0x41])
    ok=1
    for i in letters:
     if ord(i)<90 and i>='A':
        ok=0
        break
    if ok==0:
     print('fake_send')
     io.send('aa')
     continue
    else:
        io.send(''.join(letters))
        print(''.join(letters))
        text=io.recv(timeout=1)
        if b'serious' in text:
            ok=0
            io.close()
            break
        break
if ok==1:
    io.interactive()
```