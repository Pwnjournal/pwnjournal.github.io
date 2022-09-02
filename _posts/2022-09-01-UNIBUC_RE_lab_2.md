---
layout: post
title:  UNIBUC_RE_lab_2
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:
- rev
description: This lab is about figuring out what assembly language translates to in C
categories:
- UNIBUC RE
---

This lab is about figuring out what assembly language translates to in C

- 1
![](/assets/img/2022-09-01-10-26-54.png)

So test rdi,rdi and then je means that if rdi=0 it returns 0
```
if (rdi==0)
 return 0;
do 
{
    rcx+=rdx*rdx;
    rdx+=1;
}while (rdx!=rdi);

return rdx;
```
Compute the sum of squares for i->0,rdi

- 2
![](/assets/img/2022-09-01-10-41-19.png)
counter=0
while (*(i+counter)!=0)
{
    counter+=1
}
return counter

This program is strlen

- 3
  
![](/assets/img/2022-09-01-10-44-33.png)

return v_rdi+v_r8+v_drx-v_rsi-v_rcd-v_r9

- 4 

![](/assets/img/2022-09-01-11-05-36.png)
```
ulong entry(ulong param_1)
{
  long lVar1;
  long lVar2;
  if (1 < param_1) {
    lVar1 = entry(param_1 - 1);
    lVar2 = entry(param_1 - 2);
    param_1 = lVar1 + lVar2;
  }
  return param_1;
}
```

- 5
  ![](2022-09-01-11-06-21.png)
```
  undefined8 entry(ulong param_1)
{
  ulong uVar1;
  if (1 < param_1) {
    if (param_1 < 4) {
      return 1;
    }
    if ((param_1 & 1) != 0) {
      uVar1 = 2;
      do {
        uVar1 = uVar1 + 1;
        if (param_1 <= uVar1 * uVar1 && uVar1 * uVar1 - param_1 != 0) {
          return 1;

        }
      } while (param_1 % uVar1 != 0);
    }
  }
  return 0;

```  

- ex2:
get the time with assembly and syscall:

```
#choose assembly architecture
context.arch = "amd64"

#write assembly code
text = """
mov eax, 201
syscall
mov eax, 60
syscall
"""

#assemble to machine code
machine_code = asm(text)

#integrate machine code into a virtual ELF file
elf = ELF.from_bytes(machine_code)

#save ELF file to disk
elf.save("output.elf")
```

- bonus

![](/assets/img/2022-09-01-10-53-55.png)


```
fixed constant (2048/3239) 

The constant should be equal with 1/2+ arg1*c>>64/(2*arg1)

This should work for most of our values:

 C

 long stuff(long long a){
 return a1/3239;
 }

 Python
temp=0
for i in range(-5000,50000,1):
    arg1=i
    d=4880784091129696121
    a=arg1
    d=d*a
    a=d&0xFFFFFFFFFFFFFFFF
    d=d>>64
    arg1=arg1-d
    arg1=arg1>>1
    a=d+arg1
    a=a>>11
    temp2=a
    if(temp2!=temp):
        temp=temp2
        print(i)
```