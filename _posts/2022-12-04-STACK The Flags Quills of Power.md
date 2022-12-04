---
layout: post
title:  STACK The Flags Quills of Power
image: /assets/img/stack the flags 22.png
date:   2022-12-04 11:11:01
tags:
- rev
description: In this challenge we need to investigate a pcap that registered an exfiltration
categories:
- Hack The Box
---

The files can be found here (https://github.com/Pwnjournal/ctf_challs/tree/main/Hack%20the%20box%20Stack%20the%20Flags). 

The Quills of Power is a magical artifact passed down for generations in the Jaga family to be retrieved in times of great need in the face of great security vulnerabilities. Help Jaga retrieve the Quills of Power from the Vault.

Looking at the text from the binary it seems we need to find a map object:


![](/assets/img/2022-12-04-11-59-59.png)

Fortuneately we have the MAP object above. Following it there is an object that starts with PK which is the magic number of an archive.

Getting the bytes out form the PK to the first string would yield an archive that contains a python file.

![](/assets/img/2022-12-04-12-01-53.png)

The python file looks like :

![](/assets/img/2022-12-04-12-02-24.png)

Running the code we find two float numbers:

![](/assets/img/2022-12-04-12-06-03.png)

Afterwards we need to replace the hardcoded seed values with these 2 values and the executable will print the flag.

![](/assets/img/2022-12-04-12-10-54.png)



