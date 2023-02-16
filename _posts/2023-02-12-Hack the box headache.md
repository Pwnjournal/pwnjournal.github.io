---
layout: post
title:  Hack the Box rev headache
image: /assets/img/HTB-Logo.png
date:   2023-02-12 21:49:01
tags:
- rev
description: Basic rev challenge
categories:
- Hack The Box
- Rev
---


The executable has an anti-debug mechanism:

![](/assets/img/2023-02-12-16-42-49.png)

It sets the var_4 variable which is then used to execute other functions if it works out.

There are also 2 decoy flags.

![](/assets/img/2023-02-12-16-46-25.png)

The first flag will be decrypted using xor:

![](/assets/img/2023-02-12-16-47-34.png)

![](/assets/img/2023-02-12-16-55-30.png)

After setting the syscall return to be 0 as per the process not being debugged.

The second flag is hidden in the decrypted main function:

![](/assets/img/2023-02-12-17-14-34.png)

afterwards the input you just wrote is compared character with character with the real flag which is also decrypted char by char


![](/assets/img/2023-02-12-17-17-11.png)

For some reason my IDA kept crashing so I used pwndbg to finish the challenge:



 [chr(i) for i in [0x6C,0x34,0x79,0x6C,0x33,0x5F,0x77,0x34,0x73,0x5f,0x68,0x33,0x72,0x33,0x21,0x7D]]
['l', '4', 'y', 'l', '3', '_', 'w', '4', 's', '_', 'h', '3', 'r', '3', '!', '}']

HTB{l4yl3_w4s_h3r3!}

