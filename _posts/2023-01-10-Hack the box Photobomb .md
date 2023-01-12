
---
layout: post
title:  Hack the Box pentest Photobomb
image: /assets/img/HTB-Logo.png
date:   2023-01-11 21:49:01
tags:
- pentest
description: Basic pentest challenge
categories:
- Hack The Box
- Pentest
---

Description

![](/assets/img/2023-01-11-21-25-39.png)

This machine focuses on giving only 1 path of access without rabbitholes or distractions. 

Running nmap against it we only see 2 services ssh, and a webserver.

```
Starting Nmap 7.93 ( https://nmap.org ) at 2023-01-11 13:55 EST
Stats: 0:01:39 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 90.94% done; ETC: 13:57 (0:00:10 remaining)
Stats: 0:01:39 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 91.04% done; ETC: 13:57 (0:00:10 remaining)
Nmap scan report for photobomb.htb (10.10.11.182)
Host is up (0.44s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 181.18 seconds



```


Afterwards we look at the webservice :

![](/assets/img/2023-01-11-21-31-37.png)

Looking at the dependencies we see that there is a javascript file. We look at that javascript file and discover that there is a password there for the photobomb service:

![](/assets/img/2023-01-11-21-33-43.png)

Afterwards we are presented with another service.
We can interact with it by clicking on download photo:

![](/assets/img/2023-01-11-21-35-23.png)

Intercepting the traffic in Burp we see 3 parameters.
We try injecting all of them but only the file is injectable:
![](/assets/img/2023-01-11-21-36-33.png)

In order to get the root flag we run linpeas. We see that we can run a command as sudo.
![](/assets/img/2023-01-11-21-38-05.png)


Upon inspection of the file we see that find is not tied to a path so we create a find command that gives us shell and we add that path to the top of the PATH variable and we are root.

`sudo PATH=$PWD:$PATH /opt/cleanup.sh`




