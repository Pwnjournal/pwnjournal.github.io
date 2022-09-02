---
layout: post
title:  MAPLECTF_22_flag_hoarding
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:
- forensics
- steganograpy
- image for
description: Steganography image where hidden in the color layers there is some binary data.
categories:
- maplectf 22
---

Steganography image where hidden in the color layers there is some binary data.

The image is :


![](/assets/img/2022-08-31-18-43-45.png)

The first rabbit hole is that over the words and under minimal security there are some artefacts consistent with top of the word stuff. Lost a cool 20 minutes there.

Steganography tools another 20 min loss.

Then photoshop play with color properties and got the hidden code:

![](/assets/img/2022-08-31-18-45-56.png)

Translated into binary it is :
01001010 00001101 00000100 00110010 10010111 00010000 00001000 00010110 10000000 01111111 01101101 11100001 01110000 11101100 11100101 11111011 11110100 11110111 10110000 11011111 11000100 10110011 01000011 00110100 11000100 10110011 10110101 11011111 10110000 11100110 11011111 01110011 11110100 10110011 01100111 10110000 11111101 

Then using cyberchef you get: maple{tw0_D3C4D35_0f_st3g0}  

![](/assets/img/2022-08-31-18-48-34.png)