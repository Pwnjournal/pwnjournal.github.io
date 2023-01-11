
---
layout: post
title:  Hack the Box rev Up a Stream 
image: /assets/img/HTB-Logo.png
date:   2023-01-11 10:11:01
tags:
- rev
description: Basic rev challenge
categories:
- Hack The Box
---

Description

Java streams are great! Why use 10 lines of completely readable code, when you could mash it all into 67 characters? +1, PR Approved!



This executable is a little bit harder for people that do not use JAVA as their daily driver. I don't use it so there was a catch that I should've gotten.

When using JDGui to disassemble the program you will get something on the lines of :

![](/assets/img/2023-01-11-11-08-23.png)

The first thing that you have to watch for is 2 mistakes in decompilation:   
`.reduce("", (paramString1, paramString2) -> paramString2 + paramString2))` should be paramString1+paramString2 because if you leave it like this it will only get the last character twice and you need the full string.

And at the end it is almost the same string:  
`.reduce("", (paramString1, paramString2) -> paramString1 + paramString1 + "O")` will become paramString2 +"O"+ paramString1


If you want to compile it, there are some changes you will have to make in order to make it compile (on eclipse at least):

```
package upstream;
import java.util.Arrays;
import java.util.List;
import java.util.Objects;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class upstream {
	  private static Integer hydrate(Character paramCharacter) {
		    return Integer.valueOf(paramCharacter.charValue() - 1);
		  }
	  private static Integer hydrate(String paramCharacter) {
		    System.out.println(paramCharacter.length());
		    return Integer.valueOf(Character.getNumericValue(paramCharacter.charAt(0)) - 1);
		  }
		  private static Integer moisten(int paramInt) {
			System.out.println(paramInt);  
			paramInt= Integer.valueOf((int)((paramInt % 2 == 0) ? paramInt : Math.pow(paramInt, 2.0D)));
			System.out.println("moisten");
			System.out.println(paramInt);
		    return paramInt;
		  }
		  private static Integer drench(Integer paramInteger) {
				System.out.println(paramInteger);  
				paramInteger= Integer.valueOf(paramInteger.intValue() << 1);
				System.out.println("drench");
			    System.out.println(paramInteger);
			    return paramInteger;

		  }
		  
		  private static Integer dilute(Integer paramInteger) {
			System.out.println(paramInteger);  
			paramInteger= Integer.valueOf(paramInteger.intValue() / 2 + paramInteger.intValue());
			System.out.println("dilute");
		    System.out.println(paramInteger);
		    return paramInteger;
		  }
		  
		  private static byte waterlog(Integer paramInteger) {
			//System.out.println(paramInteger);
		    paramInteger = Integer.valueOf((((paramInteger.intValue() + 2) * 4 % 87 ^ 0x3) == 17362) ? (paramInteger.intValue() * 2) : (paramInteger.intValue() / 2));
		    //System.out.println("waterlog");
		    //System.out.println(paramInteger);
		    return paramInteger.byteValue();
		  }
		  private static String hexconversion(Integer paramInteger) {
				System.out.println(paramInteger);
				String finals=Integer.toHexString(paramInteger);
				System.out.println("final");
				System.out.println(finals);
				return finals;
			  }
		  
	public static void main(String[] args) {
		String str = "FLAG";
	    Objects.requireNonNull(System.out);
	    
	    List<String> step1=((List)str.chars().mapToObj(paramInt -> Character.valueOf((char)paramInt)).collect(Collectors.toList())); 
	    System.out.println(step1);
	    Stream<Character> step1array = str.chars().mapToObj(c -> (char) c);
	    String step2=(step1array.peek(paramCharacter -> hydrate( paramCharacter.toString())).map(paramCharacter -> paramCharacter.toString())).reduce("", (paramString1, paramString2) -> paramString1 + paramString2);//.reduce("", (paramString1, paramString2) -> paramString2 + paramString2)                           );
	    
	    System.out.println("ceva");
	    
	    
	    System.out.println(step2);
	    
	    
	    List<Character> step3=(List)     step2.chars().mapToObj(paramInt -> Character.valueOf((char)paramInt)).collect(Collectors.toList()) ;
	    System.out.println(step3);
	    String step4 = ((String)step3.stream().map(paramCharacter -> paramCharacter.toString()).reduce(String::concat).get());
	    System.out.println(step4);
	    List<Integer> step5 = ((List) step4.chars().mapToObj(paramInt -> Integer.valueOf(paramInt)).collect(Collectors.toList())); 
	    System.out.println(step5);
	    String step6 = (String)step5.stream().map(paramInteger -> moisten(((Integer) paramInteger).intValue())).map(paramInteger -> Integer.valueOf(paramInteger.intValue())).map(upstream::drench).peek(upstream::waterlog).map(upstream::dilute).map(upstream::hexconversion).reduce("", (paramString1, paramString2) -> paramString2 +"O"+ paramString1 );
	    System.out.println(step6);
		// TODO Auto-generated method stub
	    

	}
	  @SuppressWarnings("unchecked")
	private static List<String> dunkTheFlag(String paramString) {
		    return Arrays.asList(new String[] { });
		  }
		  

}
```

I have added a few print statements to help me understand it better.


Now all we have to do is take the file with the flag that is provided to us and reverse the operations performed:

hydrate->moisten->drench->dilute

The peek operation is not executed as you can see if you ran the program.

Having enough of JAVA i wrote a Python program in order to reverse the operations and get the flag. You can put prints after each operation to compare with the eclipse one. Also you should put the first c if you want to compare as the second is the encoded flag.

```
import math
c=[0x3b13,0x3183 ,0xe4 ,0xd2]
c=[0xb71b,0x12c,0x156,0x6e43,0xd8,0x69c3,0x5cd3,0x144,0xe4,0x6e43,0x37cb,0xf6,0x69c3,0x1e7b,0x156,0x3183,0x69c3,0x6c,0x8b3b,0xc0,0x1e7b,0x156,0xfc,0x50bb,0x69c3,0xc0,0x102,0x6e43,0xde,0xb14b,0xc6,0xfc,0xd8]

newc=[]
for i in c:
    newc.append(i*2/3)
c=newc
newc2=[]
for i in c:
    newc2.append(int(i)/2)
c=newc2
newc2=[]
for i in c:
    if i%2!=0:
        newc2.append(math.sqrt(i))
        print(math.sqrt(i))
    else:
        newc2.append(i)
flags=[chr(int(i)) for i in newc2[::-1]]
print(''.join(flags))
```

This prints `HTB{JaV@_STr3@m$_Ar3_REaLlY_Hard}`