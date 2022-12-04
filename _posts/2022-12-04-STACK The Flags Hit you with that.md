---
layout: post
title:  STACK The Flags Hit you with that
image: /assets/img/stack the flags 22.png
date:   2022-12-04 11:11:01
tags:
- forensics
description: In this challenge we need to investigate a pcap that registered an exfiltration
categories:
- Hack The Box
---

The files can be found here (https://github.com/Pwnjournal/ctf_challs/tree/main/Hack%20the%20box%20Stack%20the%20Flags). 

An adversary was observed to be exfiltrating a file from a music entertainment company that contains a to-be-released embargoed poster of a new song for one of their bands. Find the leaked file, which will contain the flag. 

Looking at the pcap we see that over 5% of the packets are not UDP/TCP:
![](/assets/img/2022-12-04-11-39-24.png)

Looking at the other protocol we see it is icmp ping.   
Browsing through the packets we see that the payload changes from packet to packet which is suspicious.
Decoding the payload of the first packet yields the flag.

![](/assets/img/2022-12-04-11-41-08.png)



