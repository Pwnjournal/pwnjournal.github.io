---
layout: post
title:  Teleport Reverse Writeup CA 2022
image: /assets/img/HTB CA 2022.jpg
date:   2022-08-29 21:52:19
tags:
- rev
description: Teleport Reverse Writeup CA 2022

categories:
- rev
---

Teleport Reverse Writeup CA 2022
You've been sent to a strange planet, inhabited by a species with the natural ability to teleport. If you're able to capture one, you may be able to synthesise lightweight teleportation technology. However, they don't want to be caught, and disappear out of your grasp - can you get the drop on them?
![](/assets/img/2022-05-19-14-41-54.png)

This exercise can either take a while or be very easy depending on your approach. I kind of took a shortcut by debugging it and then looking there your input is stored in the .bss section.
![](/assets/img/2022-05-19-17-52-48.png)
Looking at the refferences to each byte reveals the correct flag for example 
![](/assets/img/2022-05-19-17-53-39.png)

![](/assets/img/2022-05-19-17-53-57.png)
HTB{h0pp1ng_thru_th3_sp4c3_t1m3_c0nt1nuum!}