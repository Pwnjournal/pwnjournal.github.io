---
layout: post
title:  crackme learning c redxens
image: /assets/img/crackmes.jpg
date:   2022-08-29 21:28:48
tags:
- crackme
- rev
description: RedXen's crackme 
categories:
- rev
---

RedXen's crackme 

RedXen's Learning C CrackMe



Print characters
1..128 | %{"$_ $([char]$_)"}


to feed input to a file and get output from the program
Get-Content solution.txt | .\learningccrackme.exe > output.txt

This function compares the password entered with a dynamically built string and if it has a bigger char then you won
![](/assets/img/2022-07-28-23-15-38.png)
This function asks for the username
![](/assets/img/2022-07-28-23-16-23.png)
This function encrypts the name and sets the variable that you have to bypass in the password compare section
![](/assets/img/2022-07-28-23-20-51.png)

Solution:
![](/assets/img/2022-07-28-23-17-17.png)

To send solution:
Get-Content solution.txt | .\learningccrackme.exe > output.txt
