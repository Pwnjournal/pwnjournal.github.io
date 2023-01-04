---
layout: post
title:  Sunshine CTF 2022 Middle Endian
image: /assets/img/sunshine_2022.png
date:   2022-11-20 11:11:01
tags:
- forensics
description: In this challenge we need to investigate a custom formatted file
---
Sunshine CTF 2022

Challenge file (flag is the png file, hope it's the original unmodified ctf file) : https://github.com/Pwnjournal/ctf_challs/tree/main/Sunshine_ctf/middle%20endian



![](/assets/img/2022-11-20-13-05-00.png)

Here at Chompy Game Studios, we have developed a brand-new byte ordering scheme: middle endian! This transformative scheme will surely impress our shareholders! Can you reveal the secret contained within?  

(Special thanks to Oreomeister for the concept)  

We are given a hex file that has the name `flag.png.me`.

The file appears to be scrambled, as the name implies:

![](/assets/img/2022-11-20-13-06-21.png)

After looking at the name of the file we can imply that the png header and footer should be present.

So we should see something like this at the end:  

![](/assets/img/2022-11-20-13-07-16.png)

And something like this in the beginning:  

![](/assets/img/2022-11-20-13-07-45.png)

By searching at the end of our input scrambled file we can see we can form a png header by mixing the top and end most parts of the file. 

End part of the input file:


![](/assets/img/2022-11-20-13-08-51.png)

Afterwards we make a python script for demangling:

```
for i in range(int(len(a)/2)):
    
    temp=a[i*2+1]
    a[i*2+1]=a[-i*2-1]
    if i*2>len(a)/2 or -i*2-1<-len(a)/2:
        break
    a[-i*2-1]=temp
```

And we get the image:


![](/assets/img/2022-11-20-13-09-57.png)

and 
