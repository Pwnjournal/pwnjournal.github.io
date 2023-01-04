---
layout: post
title:  Hack the Box rev Behind the Scenes
image: /assets/img/HTB-Logo.png
date:   2023-01-04 18:11:01
tags:
- rev
description: Basic rev challenge
categories:
- Hack The Box
---

## Behind the Scenes

Very Easy  

After struggling to secure our secret strings for a long time, we finally figured out the solution to our problem: Make decompilation harder. It should now be impossible to figure out how our programs work!


![](/assets/img/2023-01-04-18-20-55.png)

The first thing we see when opening the file with a dissasembler is the signal activity in the main function:

![](/assets/img/2023-01-04-18-36-54.png)

Then looking further into what the ud2 assembly instruction does we get the following:  

UD2--Undefined Instruction  
Generates an invalid opcode. This instruction is provided for software testing to explicitly generate an invalid opcode.   

So it will trigger an exception when executed.

Further let's see what the signal is that is handled (4) the page i found is (https://www.javatpoint.com/linux-signals) and the entry said:

4. 	SIGILL 	It is an Illegal instruction. There is some machine code that is contained in the program, and the CPU cannot understand this code.

When debugging the executable when the error is met it simply jumps to the next opcodes (+2) as per the handling function:

![](/assets/img/2023-01-04-18-50-05.png)

Next we follow the execution chain without needing to debug in order to understand the program.

1. It checks for 2 arguments (1 by default and 2nd given by us ).
2. It checks the keyword argument length (0xC= 12 characters)
3. compares the first part with Itz
4. compares afterwards with _0n
5. compares afterwards with Ly_
6. compares with UD2

Itz_0nLy_UD2

Passing the phrase gives us the flag HTB{Itz_0nLy_UD2}
