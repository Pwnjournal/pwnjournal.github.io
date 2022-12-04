---
layout: post
title:  STACK The Flags Cobalt Struck 
image: /assets/img/stack the flags 22.png
date:   2022-12-04 11:11:01
tags:
- forensics
description: In this challenge we need to investigate an infection and find 3 flags from initial access -> execution -> persistence
categories:
- Hack The Box
---

The files can be found here (https://github.com/Pwnjournal/ctf_challs/tree/main/Hack%20the%20box%20Stack%20the%20Flags). Mount the vhd in a vm as to not trigger the av, and be sure to snapshot before as to revert if infecting yourself.

Peter's company, Spidy Inc. had suffered a data breach and had sensitive information stolen from his Internal file server, which can only be accessed from Peter's laptop. Based on the information provided by Peter, he had bought a new laptop and installed a few software to set up the laptop. Based on some other evidence, it is determined that Peter's laptop could be the patient zero for the cyber attack. As a member of an investigation team, you have been tasked to investigate activities related to  
1. Initial Access  
2. Execution  
3. Persistence  
The laptop image has been acquired for your analysis. Find out what the adversary did to maintain persistence when initial access and execution are achieved.  
The flag consists of 3 parts. Concatenate these 3 parts to form the flag.  
The password is STF22.  


The things we need to do is analyse the Event Logs, the wmi repo, the downloads folder, registry, tasks folder, mft etc..

The first obvious part of the flag is found in the Documents folder of the user Peter. It is an executable file p.exe that has the image of an Office installer.

![](/assets/img/2022-12-04-11-24-21.png)

Running strings against it reveals the first part of the flag.

P.a.R.t_1 U1RGMjJ7UGVyczFzdDNuYzNfR

The next part of the flag is done by analysing the SAM registry hive for the user eviluser

Running either regripper or Registry explorer and then looking at the registry values for the eviluser user would yield the second portion of the flag:

![](/assets/img/2022-12-04-11-29-19.png)

P.a_R_T_Two jByX1IzbTB0M19DMG50cjBsXz

The final part of the flag will be found in the tasks folder as a task.


![](/assets/img/2022-12-04-11-31-11.png)

P_a.R.T_III R0X1kwdXJfRjFuZzNyX1QxcHN9

Concatenating the Base64 codes and decoding yields the flag.


