---
layout: post
title:  Hack the Box rev Simple Encryptor
image: /assets/img/HTB-Logo.png
date:   2023-01-04 18:11:01
tags:
- rev
description: Basic rev challenge
categories:
- Hack The Box
---

## Simple Encryptor

Very Easy  

CHALLENGE DESCRIPTION

On our regular checkups of our secret flag storage server we found out that we were hit by ransomware! The original flag data is nowhere to be found, but luckily we not only have the encrypted file but also the encryption program itself.

The main function looks like this:

![](/assets/img/2023-01-04-20-03-33.png)

The file is encrypted with xor of a random number byte by byte, the rest of the ops are reversible.
The seed is saved in the first 4 bytes of the encrypted file.

We need to find a way to call the c dll from python.
For this we use the ctypes library as so:

from ctypes import *
libc=cdll.LoadLibrary("libc.so.6")  
libc.srand(seed)
libc.rand()

Our decryption function would look like:

```
file=open("flag.enc",'rb')
content=file.read()
ror = lambda val, r_bits, max_bits: \
    ((val & (2**max_bits-1)) >> r_bits%max_bits) | \
    (val << (max_bits-(r_bits%max_bits)) & (2**max_bits-1))
 
max_bits = 8
new_content=[]
seed=unpack(content[:4], 32)
libc.srand(seed)
print(hex(seed))
for i in range(len(content)-4):
 first_rand=libc.rand()&0xFF
 print(hex(first_rand))
 second_rand=libc.rand()&7
 print(hex(second_rand))
 new_content.append(ror(content[4+i],second_rand,max_bits)^(first_rand))

print(bytearray(new_content))
```


which gets us the flag:  


HTB{vRy_s1MplE_F1LE3nCryp0r}

The operations have to be done in reverse. The correct order would be:
1. we do the inverse of rotate left which is rotate right
2. we xor the bytes with the first random

Afterwards the string is decrypted.