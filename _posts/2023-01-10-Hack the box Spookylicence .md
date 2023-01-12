
---
layout: post
title:  Hack the Box rev Spookylicence 
image: /assets/img/HTB-Logo.png
date:   2023-01-12 11:15:01
tags:
- rev
- angr
description: Basic rev challenge
categories:
- Hack The Box
---

Description

After cleaning up we found this old program and wanted to see what it does, but we can't find the licence we had for it anywhere. Can you help?



The file has a long chain of compare statements in it:

![](/assets/img/2023-01-12-11-17-12.png)

It would take a long time to solve this chain by hand, so the best way here is using angr.

This tool takes an address and it calculates the path to get to that address in an executable. It helps that the flag length is known (32) and that the path is kind of linear, without rabbit holes.

Angr loads the binary at address 0x400000 so we need to rebase the program in IDA as well:

we go to Edit->Segments->Rebase program and set the new base for this program.

Then we get the `you win` address:


![](/assets/img/2023-01-12-11-24-03.png)


Make the script

For me it did not run on kali and I had to use Ubuntu to run it correctly, it gave me a weird error on kali which i did not find online.

```
import angr,claripy
proj = angr.Project('spookylicence')
flag=claripy.BVS('flag',8*32)
state=proj.factory.full_init_state(args=['spookylicence',flag])
simgr = proj.factory.simulation_manager(state)
simgr.explore(find=0x401876) 
print(simgr.found[0].solver.eval(flag, cast_to = bytes))

```
![](/assets/img/2023-01-12-11-27-14.png)

`HTB{The_sp0000000key_liC3nC3K3Y}`