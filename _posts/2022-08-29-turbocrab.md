---
layout: post
title:  turbocrab
image: /assets/img/CORCTF 2022.PNG
date:   2022-08-29 21:52:19
tags:
- rev
description: This challenge is about rust binary that executes shellcode and how you remove and decrypt the flag

categories:
- rev
---

This challenge is about rust binary that executes shellcode and how you remove and decrypt the flag



Run the executable gather every output string search each string and see if it matches



![](/assets/img/2022-08-11-16-26-20.png)

We find the flag string



convert only the Flag... to string:


![](/assets/img/2022-08-11-16-27-53.png)

See references:


![](/assets/img/2022-08-11-16-28-16.png)

We see an if else statement and the else statement leads to the flag variable:



![](/assets/img/2022-08-11-16-30-08.png)

We also see that the variable which is compared is the result of calling a memory location and that the program calls memoryprotect which is used when you want to execute something you just wrote.



Further back we see the encrypted flag


![](/assets/img/2022-08-11-16-31-32.png)

As an additional obvious hint the function is called shellcode



What i wanted to do next was debug the process. Somehow ida didn't work with the debug server (won't get to the breakpoint) but sanity check with gdb did so issuing the gdb server command :


`gdbserver 0.0.0.0:23946 turbocrab`


and then connecting with ida worked better


![](/assets/img/2022-08-11-17-11-43.png)

We can see something is xored with 0x13
The first function is a syscall to read


Then i had another problem because the memory would show up as 0s even though i know they aren't so i decided to use gdb


Although the operation is hidden, The operation that we needed is here:

![](/assets/img/2022-08-11-17-37-07.png)


sub al,0x1e the rest is bullshit.

Also the xor earlier.  
So we need to xor and sub but backwards which is add and xor:   
 

![](/assets/img/2022-08-11-17-40-28.png)


The flag looks schetchy but readable  

Using the fact that we can see some words such as vm and reversing we just need to match the hash:



"dc136f8bf4ba6cc1b3d2f35708a0b2b55cb32c2deb03bdab1e45fcd1102ae00a"

We bruteforce the remaining characters untill we get the flag.



corctf{x86_j1t_vm_r3v3rs1ng?}

