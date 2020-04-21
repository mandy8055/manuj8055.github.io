---
title: Random Numbers in Java
tags: [java, Random Numbers]
style:
color:
description: Basics of Random numbers generation in java
---
<a class="text-center" href="https://feedburner.google.com/fb/a/mailverify?uri=Mandy8055&amp;loc=en_US" onclick="window.open(this.href, 'subscribe',
    'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;">Subscribe for New Posts</a>

Most of the commercial applications require tokens which needs to be generated at fixed intervals of time. This is the reason why Random number generation is very important tool for any software developer. They are used in banking applications, e-commerce applications, lottery ticket generation, etc. Java provides support for generating random numbers primarily using [java.lang.math](https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html) and [java.util.Random](https://docs.oracle.com/javase/8/docs/api/java/util/Random.html) 

### Random Numbers using Math class

The Math class contains static `random()` method which generates random numbers in the range **[0.0, 1.0)**. When we call the `Math.random()` method; internally a `java.util.Random` pseudonumber generator object is created and used.

#### Generating random Numbers within a given range
The standard expression for generating random Numbers within a given range(in integer) is:
```java
(int)(Math.random() * ((maxRange - minRange) + 1)) + min;    
```

##### Breaking the above expression down
1. Multiply the magnitude of random number(which we got from Math.random()) with the range of numbers you want to calculate`(maxRange - minRange + 1)`. Notice here the **max range is excluded** from the answer. For example, say Math.random() gave **0.5**. We want to generate random numbers between **20 and 30**. Therefore, ((30 - 20) + 1) = 5.5. 
2. Shift the range up to the range you are targeting by add the result with **min** i.e. 5.5 + 20 = 25.5
3. The return type of the above expression is **double**. So, the answer which you'll get will still be in double. To get the integer result you **need to type cast it** i.e. we got 25.

**NOTE:** You can pass the range of negative values to **generate negative random numbers** in the given range. 

### Random Numbers using the Random class

To generate the random numbers using **Random** class method;
1. Create an instance(object) of Random class.
2. Call one of the random value generator method such as `nextInt()`, `nextDouble()`, or `nextLong()`.

```java
import java.util.Random;
public static int generateRandomInteger(int uLimit){
    // Create an instance
    Random random = new Random();
    // Calling random value generator method 
    return random.nextInt(uLimit);
}
```
**_The above code generates random numbers within the range [0, uLimit)._**

```java
import java.util.Random;
public static int generateRandomInteger(int lLimit, uLimit){
    // Create an instance
    Random random = new Random();
    // Calling random value generator method 
    return random.nextInt((uLimit - lLimit) + 1) + lLimit;
}
```
**_The above code generates random numbers within the range [lLimit, uLimit)._**

### Random Number generation in Java 8
Java8 introduced a new method in the java.util.Random called **`ints()`**. This method returns unlimited stream of pseudorandom int values. We can limit the random numbers between a specified range by providing the minimum and maximum values. Read more about in **[here](https://docs.oracle.com/javase/8/docs/api/java/util/Random.html).**

### Conclusion
Mostly all the random generator throughout languages implements [linear Congruential Generator(LCG)](https://youtu.be/PtEivGPxwAI)algorithm. This algorithm is fast in generating random numbers but it does not work exactly the way random numbers should be generated. Please read about pseudo-random numbers and true-random numbers with their distinction in [**here.**](https://www.researchgate.net/post/Difference_between_TRNG_or_PRNG#:~:text=The%20difference%20between%20true%20random,(completely%20computer-generated)) So, this is not very efficient for generating random numbers in real-time scnearios like cryptography, lotteries, unique transaction id generation. [SecureRandom](https://docs.oracle.com/javase/8/docs/api/java/security/SecureRandom.html) and [XorShift](https://www.javamex.com/tutorials/random_numbers/xorshift.shtml#.Xje2fKbhUXd) random generators are basically used in real-time scenarios. Among them, xorshift method is not so much popular as the random numbers which will be genrated in future can be guessed by looking at some previous random numbers generated. For Real-time Random Numbers generation and unique ids in java, please read **[this](https://mandy8055.github.io/blog/generating-unique-ids-java)** blog.

There is one more interesting resource which I got recently. You can read about it at **[random.org](https://random.org/)** which generates true random numbers that are generated via **atmospheric noise**. 

