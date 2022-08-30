---
layout: post
title:  Extracting an object from memory using Ida
image: /assets/img/IDA tricks.jpg
date:   2022-08-29 21:28:48
tags:
- rev 
- ida
description: Extracting an object from memory using Ida Pro
categories:
- rev
- ida
---
Extracting an object from memory using Ida Pro
For the purpose of this tutorial I will be 
object beginning 
![](/assets/img/2022-05-19-23-35-47.png)
object length
import ida_bytes
data = ida_bytes.get_bytes(0x55CD3E98F722, 0x752)
file_descriptor = open('dump_space', 'wb')
file_descriptor.write(data)
file_descriptor.close()


This will save your input to a file.
If you want to see what code you have selected use:
import idc
print(hex(idc.read_selection_start()))