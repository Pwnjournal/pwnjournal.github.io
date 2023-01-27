---
layout: post
title:  Hack the Box pentest Soccer
image: /assets/img/HTB-Logo.png
date:   2023-20-02 15:49:01
tags:
- pentest
description: Basic pentest challenge
categories:
- Hack The Box
- Pentest
---

Description

![](2023-01-12-20-38-09.png)


This machine focuses on giving only 1 path of access without rabbitholes or distractions. 

Running nmap against it we only see 2 services ssh, and a webserver.

```

```

We run gobuster and see that tiny is a working path for the executable.

![](2023-01-12-20-39-52.png)