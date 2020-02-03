---
title: Big Integers in Java
tags: [java, BigIntegers]
style:
color:
description: Big Integers and their importance in Competitive Coding
---
<a class="text-center" href="https://feedburner.google.com/fb/a/mailverify?uri=Mandy8055&amp;loc=en_US" onclick="window.open(this.href, 'subscribe',
    'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;">Subscribe for New Posts</a>

Competitive coding has always been a fun part for me. I strongly believe that that everyone irrespective of the computer science background should do competitive coding. It is just like playing Chess. You can play chess everyday just to test your logical limits and enhance it. I'll try to draw a little comparison. Just like in Chess you need to learn the moves of the every pieces; in competitive coding you need to know a programming language that will help you to start and enjoy the essence of programming. Only this part is little time-consuming. Rest is same. You practice, you loose; you practice, you improve; you practice, and finally you win.

I prefer to use java when it comes to competitive coding, since I trained myself in the same. It is always important to know the ins and outs of the toolbox which you're using. **BigIntegers** are a boon for every java programmer when it comes to competitive coding. Mostly every problems involve some kind of data constraints and there are hidden test cases which try to test the logic and code written by you. Sometimes, although your logic is right but the constraints will fail some of the test cases. When it comes to dealing with constraints with relatively humongous values, as a java programmer it is important to know the concept of BigIntegers and the functionalities which it offers. It is a great and must tool which needs to be added to a java developer's toolbox.

BigInteger class is used for mathematical operations which involves very large integer calculations. These operations are way beyond the limit of primitive data types. Some instances can be like, calculating large fibinacci numbers, factorial of 100, etc which contains 158 digits. It is impossible for any primitives but in BigIntegers it is possible. The reason being an instance of BigInteger is created, and as we know that objects are created in Heap or dynamic allocation, so there is no limit on the upper bound of range. Although we know practically, you cannot have infinite memory so in this case **you can store a number that has `Integer.MAX_VALUE` number of bits**.

### Basics and Important methods which are frequently required

**1. Creating and initializing BigIntegers**
```java
import java.math.BigInteger;
BigInteger bigInt = new BigInteger("0");// zero is BigInteger.ZERO
BigInteger bigInt1 = new BigInteger("12"); // Any value you want to initialize(12 for example)
BigInteger bigInt2, bigInt3;
int i = 23;
bigInt2 =  BigInteger.valueOf(i);
bigInt3 = BigInteger.valueOf(23);
``` 

**2. Some Basic Operations on BigIntegers**
```java
// Analogues to c = a + b;
C = A.add(B);
C = A.subtract(B);
C = A.multiply(B);
C = A.divide(B);
```

**3. Converting the value from conventional types to BigIntegers**
```java
String str = "123467";
C = A.add(new BigInteger(str));
int i = 23;
C = A.add(BigInteger.valueOf(i));
```

**4.Converting the value from BigIntegers to conventional types** 
```java
String str = A.toString();
int i = A.intValue();
long b = A.longValue();
double c = A.doubleValue();
```

### Frequently used and important Methods og BigInteger Class
**1. Bit Manipulations**
- and()
- andNot()
- bitCount()
- bitLength()
- clearBit()
- flipBit()
- lowestSetBit()
- shiftLeft()
- shiftRight()
- setBit()
- testBit()
- xor()
- or()
- not()

**2.Comparisions**
- compareTo()
- equals()

**3. Arithmetic**
- add()
- subtract()
- multiply()
- divide()
- pow()
- sqrt()
- abs()
- max()
- min()
- negate()
- remainder()
- gcd()

**4.Modular Arithmetic**
- mod()
- modInverse()
- modPow()

### Conclusion

The **BigInteger** class is really a life saver in many situations and instances when it comes to Competitive programming. However, everything good comes with a cost too and so are BigInteger. These operations are not constant operations as in the case with primitive types. For instace consider the addition of two numbers. If we talk about complexity addition of two numbers(using primitive types) is always O(1) but when it comes to BigInteger that is not the case. **It depends upon the number of digits of the numbers we are taking into consideration.** That is one point to take take care while using BigIntegers. To sum up, BigIntegers are really a great tool to have in your coding toolbox.  

