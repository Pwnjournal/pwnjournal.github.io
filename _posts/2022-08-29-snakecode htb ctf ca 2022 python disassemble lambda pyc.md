---
layout: post
title:  snakecode htb ctf ca 2022 python disassemble lambda pyc
image: /assets/img/HTB CA 2022.jpg
date:   2022-08-29 21:52:19
tags:
- rev
description: Snakecode write-up ca 2022

categories:
- CA HTB CTF
---

Snakecode write-up ca 2022
We found this ancient text inscribed on a stone tablet. We believe it describes the history and technology of a mighty but extinct civilization, but we can't be certain as it's written in a dead language. Can you use your specialist knowledge to uncover the truth, and recover their technology?

The file is a pyc file which can be decompiled with uncompyle6 resulting in:


###### uncompyle6 version 3.7.4
###### Python bytecode 2.7 (62211)
###### Decompiled from: Python 2.7.18 (v2.7.18:8d21aa21f2, Apr 20 2020, 13:25:05) [MSC v.1500 64 bit (AMD64)]
###### Embedded file name: ./snake_obf.py
###### Compiled at: 2022-01-18 00:16:46




<code>import marshal, types, time
ll = types.FunctionType(marshal.loads(('YwEAAAABAAAABQAAAEMAAABzNAAAAHQAAGoBAHQCAGoDAHQEAGQBAIMBAGoFAHwAAGoGAGQCAIMB\nAIMBAIMBAHQHAIMAAIMCAFMoAwAAAE50BAAAAHpsaWJ0BgAAAGJhc2U2NCgIAAAAdAUAAAB0eXBl\nc3QMAAAARnVuY3Rpb25UeXBldAcAAABtYXJzaGFsdAUAAABsb2Fkc3QKAAAAX19pbXBvcnRfX3QK\nAAAAZGVjb21wcmVzc3QGAAAAZGVjb2RldAcAAABnbG9iYWxzKAEAAAB0AQAAAHMoAAAAACgAAAAA\ncwcAAAA8c3RkaW4+dAoAAABsb2FkTGFtYmRhAQAAAHQAAAAA\n').decode('base64')), globals())
i0 = ll('eJxLZoACJiB2BuJiLiBRwsCQwsjQzMgQrAES9ythA5JFiXkp+bkajCB5kKL4+Mzcgvyikvh4DZAB\nCKKYHUjYFJekZObZlXCA2DmJuUkpiXaMEKMZGAC+nBJh\n')
i1 = ll('eJxLZoACJiB2BuJiLiBRwsCQwsjQzMgQrAES9ythA5LJpUXFqcUajCB5kKL4+Mzcgvyikvh4DZAB\nCKKYHUjYFJekZObZlXCA2DmJuUkpiXaMEKMZGADEORJ1\n')
f0 = ll('eJxLZmRgYABhJiB2BuJiXiBRw8CQxcCQwsjQzMgQrAGS8ssEEgwaIJUl7CAiMzc1v7QEIsAMJMoz\n8zTASkBEMUiJTXFJSmaeXQkHiJ2TmJuUkmgHVg5SAQBjWRD5\n')
f1 = ll('eJxLZmRgYIBhZyAu5gISNQwMWQwMzQwMwRogcT8wWcIKJNJTS5IzIFxmIFGemacBpBjARDE7kLAp\nLknJzLMr4QCxcxJzk1IS7cDKQSoAvuUPJw==\n')
f2 = ll('eJx1kL1uwkAQhOfOBsxPQZUmL+DOEnWUBghEQQbFIESVglUkY5ECX+lHoMz7Jrt7HCgSOWlGO/rm\n1tbtIwBBY1b9zdYYkEFlcRqiAQoWxaginDJhjcUBijNQy+O24jxgfzsHdTxOFB8DtoqPoK7HPcXn\ngCPFZ1BfcUGsdMA/lpc/fEqeUBq21Mp0L0rv/3grX/f5aELlbryVYzbXZnub7j42K5dcxslym7vu\nJby/zubrK1pMX9apPLOTraReqe9T3SlWd9ieakfl17OTb36OpFE/CDQDE5vHv7K/FKBNmA==\n')
f3 = ll('eJxVj00KAjEMhV+b8Q9040IZT9C9WxHEvRvBC1iFUhhk2sUIIwgexLWn1KQzI9qSl/DlhaZHDSDj\nII4tR3ix1IBVyK1GXitImt/0l1JDSSih1rAZfIZyI4x9BRIkeKA8SLeF1Dl9clIHG+c9OakdZ35O\nT/o+yiciZI2Hgvpt702Pt925Nx/HFZwSGbIYqaL87FS5aKSIgi5JbZR/F1WTrkZmk4QByypE64p1\nap6X4g8LaaoZ3zFGfzFVE/UBTuovhA==\n')
f4 = ll('eJw1zDsKgEAMBNCJilb2drZ7AEuxsbfxBOIHFFkWNqWdF3eyYJEXkgxZcwB/jazYkkdwUeAVCAcV\nW3F4MjTt7ISZyWVUS7KEsPtN7cW9e2ddLeKTIXk7gkSsSB91O/2g9uToLBELO0otH2W6Ez8=\n')
f5 = ll('eJxdjr0OwjAMhM9J+as6M7HTF0AsiKV7F54ACJUKVaiSjOnEi2MbISQGf4rtu3OuMwBSBVfDFQdG\nBhzwMAgNMsER1s58+wJ3Hlm4Ai/z33YGE+A1IrNljnBBtiLYT1ZSf2sr6lMt19u+ZPYQkGDJqA0j\nycfap7+lBT/C2bveJ/UkEQ7KqByTGMbPKNQSpojiPMTEzqNKup2aKlnShramopJW5g2ipyUM\n')
f6 = ll('eJxdjTEOglAQRB98iMbEKxhLbkBjaLSwsrHWBEUJCRKULTT5VFzc3W9nMS+zk93ZqwNS1UK1VQ17\nRQ0CVcQUsTvljO4vWjEmSIRP8A4PXn3MlHKOea4DlxyzWMsOjXUHK/bpVXb1TWy855kF2gN9SPo2\nDD9+At8Zdm4YZorNIFXTFTI335aPS1UWtie28QV3xx4p\n')
f7 = ll('eJxtjz8LwjAQxV/S1mrRxcnZKat/qyAuOrv0E4ilIJRS2hsUCg7OfmcvubZTIe/97nKPcHkEADpd\nWPWPjYCGj0Kj0fjIfHwVqiWIbzxbJ6SHEleQ1yf8ocQHFLSJqgKN+nTYVUUEGndNCiRG8UY3M7F7\nabb7TrAS7AVrQSw4CDaCreBo7CfJPvdy/nZeummZuyY3bHBWh2ynmtJncXaRLLaJem6HaqGiVlMV\n6Zn+Azn/L1k=\n')
f8 = ll('eJwljr0KAkEMhCf3o2hrIb7BlWIhFiKC1jYWViKHe+qKnHob0GKt7sVNcsV8ZDeTSc45gJ5oINqI\nwkkQgTvQAvRdgwmO0BK2xxl+uTUTxBwugUtxT8EZIiHKZ4o21dZE7FLRe4yD+nMLixlchvG+0KU7\nPxR6EVjhSVDoKazt86MqG6uasr5WrI3SucCNbJPEp685keIy576aqktThVs3r0kf48s8r4c9Ogaj\nL3SnIej8MrDz9aqLXJhPzwMNaURT4R/aUC0X\n')
a1 = ll('eJxLZmRgYIBhZyAuZgESKYwMwRpMQIZfCUhcWwNIMGiAmGB+DoQPIorZgYRNcUlKZp5dCQeInZOY\nm5SSaAdWDFIBALI0C1U=\n')
a2 = ll('eJxLZmRgYIBhZyAuZgESKYwMwRpMQIZfCUhcWQNIMGiAmGB+DoQPIorZgYRNcUlKZp5dCQeInZOY\nm5SSaAdWDFIBALBMC00=\n')
a3 = ll('eJw10EtLw0AUBeAzTWLqo74bML8gSyFdiotm40rEZF+kRyVtCGKmqzar/nHvHBDmfty5c+fBrB2A\niUVuUVkMG4MOnIARGIMJeAKm4BQ8Bc9UsfwcvABn/5VL8Aq81tINeAveKb/Hd47R4WDDTp5j7hEm\nR4fsoS4yu+7Vh1e8yEYu5V7WciffZCl/5UpW8l162cuF3Mq1fJSUY5uYhTZFRvfZF+EvfOCnU89X\ngdATGFLjafBs+2e1fJShY4jDomvcH1q4K9U=\n')
a4 = ll('eJxLZmRgYIBhZyAuZgESKYwMwRpMQIZfCUhcRQNIMGiAmGB+DoQPIorZgYRNcUlKZp5dCQeInZOY\nm5SSaAdWDFIBALCJC04=\n')
a5 = ll('eJxNzTELwjAQBeCXS4r6TzKJP6DUgruLq0s1S7BKIRkqJP/dd3Hp8D4ex3H3NAA6xjEXJo2kAHeH\nalAF1aI6FINg8BIsZxTZdM5lM2/95i2PXCNBPBCvzeubLOR4yvp2bX6bS3P+LyppR/qUQ/wMea99\nnt6PMA26l/SKxQ/XGxky\n')
a6 = ll('eJwlzLsKwkAQheF/L0afw2qr4AOENOnT2NpEgyDGENgtFHbfPTNrcT6G4cw8DHCQeMkgiWchw81T\nDMVSHMWTDdnytGTHu+Ea9G4MAkHPkxXaS9L1t/qrbtXlX1TiUehiml9rn046L9PnPk+99qJ+cewN\nxxM9\n')
a7 = ll('eJwlzLEKwjAQxvF/rhF9jk6Zig8gXdy7uLq0FqFYRUiGFpJ39y4O34/j+O4eDjhovOaqia2S4e4p\njiKUhuLJjiw8hex5Cbdgd0NQCHaeROnOydZbda9+q+u/aMSjcolpXj59Otm8ju9pHnvrRfvS8AMM\nqhM6\n')
a8 = ll('eJxLZmRgYABhJiB2BuJiPiBRw8CQwsgglsLEkM3EEKzBDBTyy2QFkplAzKABJkCaSkBEjgZcsJgd\nSNgUl6Rk5tmVcIDYOYm5SSmJdmDFIBUAVDAM/Q==\n')
a9 = ll('eJxLZmRgYIBhZyAuZgESKYwMwRpMQIZfCUhcQQNIMGiAmGB+DoQPIorZgYRNcUlKZp5dCQeInZOY\nm5SSaAdWDFIBAK+VC0o=\n')
m0 = ll('eJw1jTELwjAUhC9Jq/0VzhldBAfr4u7i6mYpFFSKCXRJp/7x3rsi5L5Avnsvrx0AS8PcmNQSGSg8\nDsWjBJQKS42nxwzMQSog09b/gsrs9AGP6LjhHr3tMfSn7TpH+yebfYtJHGXH7eknTpGAkPbEJeVu\n+F5V/Bw1Wpl0B7cCYGsZOw==\n')
m1 = ll('eJw1zUEKAjEMBdCfdMQreIRuRwU3Mhv3bjzCDAOCitCAm7rqxU1+cZGX0v408wbAvy5e5eQYUAUm\nqAnNHdASvsJLhSVUBpryoPG6Km5ZfPaah/hBnXXf29jbsbdDjl0W2Tdd6IN+6JwdkLJ1zsWW+2vi\n/HOMRIklkJ38AF2QGOk=\n')
m2 = ll('eJxNjj8LAjEMxV96fz+Fk0NHdT5c3F1cD5c7BEHlsAWXdrov7kuKICS/0LyXpFMP4JcnZrgSEUgM\nQXJIDVKLtcHokAWZKvsVUm0eGjr1rC3GCplBW/03Xpy2hM5bj4sXnjh7p4cUz30pO6+fiKouxtn6\ny8MehcH4MU7GtydgCB0xhDjfX8ey8mAzrYqyka18AW5IIKw=\n')
def snake(w):
    r = i0()
    c = i1()
    f0(w)
    d = (0, 1)
    p = [(5, 5)]
    pl = 1
    s = 0
    l = None
    while 1:
        p, d, pl, l, s, w, c, r = m2(p, d, pl, l, s, w, c, r)
        time.sleep(0.4)
    return
i1().wrapper(snake)</code>


The challenge is on reversing the lambda functions that follow.
My first attempt was at decompiling the ll function that is used to load all the other functions. But the other functions are already compiled as well so my first plan failed. Then while testing the snake i saw that it fed me letters randomly so i decided to disassemble all the functions to see the logic of the game.

I deciphered most of it before i saw that the flag was hiding into one of the lambda functions.
I tried 2 methods first didn't work but still might work for you:
method 1:
from lamtest import myfunc
import inspect
inspect.getsource(myfunc)
method 2:
import dis

<code>>>> dis.dis(a3)
  2           0 LOAD_CONST               1 ('H')
              3 LOAD_CONST               2 ('T')
              6 LOAD_CONST               3 ('B')
              9 LOAD_CONST               4 ('{')
             12 LOAD_CONST               5 ('S')
             15 LOAD_CONST               6 ('u')
             18 LOAD_CONST               7 ('P')
             21 LOAD_CONST               8 ('3')
             24 LOAD_CONST               9 ('r')
             27 LOAD_CONST              10 ('_')
             30 LOAD_CONST               5 ('S')
             33 LOAD_CONST               8 ('3')
             36 LOAD_CONST              11 ('C')
             39 LOAD_CONST              12 ('R')
             42 LOAD_CONST              13 ('t')
             45 LOAD_CONST              10 ('_')
             48 LOAD_CONST               5 ('S')
             51 LOAD_CONST              14 ('n')
             54 LOAD_CONST              15 ('4')
             57 LOAD_CONST              16 ('k')
             60 LOAD_CONST               8 ('3')
             63 LOAD_CONST              17 ('c')
             66 LOAD_CONST              18 ('0')
             69 LOAD_CONST              19 ('d')
             72 LOAD_CONST               8 ('3')
             75 LOAD_CONST              20 ('}')
             78 BUILD_LIST              26
             81 STORE_FAST               1 (f)
  3          84 LOAD_FAST                1 (f)
             87 LOAD_FAST                0 (pl)
             90 LOAD_CONST              21 (5)
             93 BINARY_DIVIDE
             94 LOAD_GLOBAL              0 (len)
             97 LOAD_FAST                1 (f)
            100 CALL_FUNCTION            1
            103 BINARY_MODULO
            104 BINARY_SUBSCR
            105 RETURN_VALUE
</code>