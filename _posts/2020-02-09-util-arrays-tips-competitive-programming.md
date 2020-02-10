---
title: Useful Arrays class methods for Competitive Programming
tags: [java, Competitive Programming, Arrays]
style:
color:
description: Few best practices while dealing with Arrays problems in Competitive Programming using java.
---
<a class="text-center" href="https://feedburner.google.com/fb/a/mailverify?uri=Mandy8055&amp;loc=en_US" onclick="window.open(this.href, 'subscribe',
    'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;">Subscribe for New Posts</a>

This post is based on my experiences of using util.Arrays class's methods for competitive programming. Competitive programming deals with problems of various plethoras which includes discrete mathematics, Data Structures and implementations, etc. **Arrays** are one of the most important data Structures that are used almost everywhere across the virtual world. It finds its most important role in competitive programming too. So, it is important to utilize some of the basic toolbox from language in which you're coding which might facilitate in easing your task while dealing with these problems.

I'll discuss some of the things which I learnt while doing competitive coding and dealing with arrays problems. Initially, I thought that using the built-in methods(which implement some of the general-purpose algorithm) will not help stregthen my concepts. And this notion was right. In early days of programming when you need to learn how these algorithms work internally; you shouldn't use any of the inbuilt methods provided by the languages. To make things clear on general-purpose algorithms; we can take examples like **_searching, sorting, copying, String matching, etc_.** **You should code out these methods yourself to get an edge of how these algorithms work.** However, once you're pretty clear on how these algorithms work behind the secenes, you should start looking for language support since the competitive challenges are timed.

Basic util.Arrays methods which are frequently used in Competitive coding:
1. asList() and toArray()
2. sort()
3. binarySearch()
4. fill()
5. equals()
6. copyOf() and copyOfRange

## Some of the best practices of using util.Arrays
Below are some of the common functionalities we use frequently while solving some real-life problems and competitive problems when dealing with arrays. 

#### 1. Coverting array to ArrayList and vice-versa
- **To convert an Array to ArrayList:**

```java
import java.util.Arrays;
Arrays.asList(yourArr);
```
- **To convert an ArrayList to an Array:**
This can be done in numerous ways. Let us discuss them one by one.

1. **Object[] toArray():**

```java
import java.util.ArrayList;
import java.util.List;
Object[] obj = givenList.toArray();
``` 
**Note: toArray() method returns an array of type Object(Object[]). We need to typecast it to Integer before using as Integer objects. If we do not type cast we will get compilation error.**

2. **T[] toArray(T[] arr):**
- Converts a list into an array.
- If array is not big enough, then new array is created and returned.
- Throws **ArrayStoreException** if the runtime type of a is not a supertype of the runtime type of every element in this list.

```java
import java.util.ArrayList;
import java.util.List;
Integer[] arr = new Integer[givenList.size()];
arr = givenList.toArray(arr);
```
Note: If the specified array is null for eg. `Integer[] arr = null;` then it will throw **NullPointerException**.

3. **Using loop and get() method:**

```java
import java.util.List;
import java.util.ArrayList;
Integer[] arr = new Integer(givenList.size());
for(int i = 0; i < givenList.size(); i++){
//    Copying the content of list in Integer[] arr.
   arr[i] = givenList.get(i);
}
```

#### 2. Using sort() method to sort the arrays:
- Sorting the array in Ascending order(default):

```java
import java.util.Arrays;
Arrays.sort(yourArray);
```
- Sorting the array in Descending order:

```java
import java.util.*;
Arrays.sort(givenArray, Collections.reverseOrder());
```
**NOTE: Please refer [this](https://github.com/RyanFehr/HackerRank/blob/master/Algorithms/Implementation/Electronics%20Shop/Solution.java) amazing problem.**

#### 3. Using binarySearch() method to search an element in the given Array:

As the name suggests; this method is pretty straight forward. It uses binary search algorithm to search a given key inside an array. There are two variants of `binarySearch()` method provided by Java language support. Let us see them and their usages.

- **Seaching for the given key within the given array:**

```java
import java.util.Arrays;
int[] arr = {20, 10, 4, 11, 54};
int key = 11;
// Always remember to sort the array before searching; since for applying binary Search
// arrays must be sorted. Also, if original index of the array is required to calculate; 
// then you have to go for linear search.(the naive approach)
Arrays.sort(arr);
int index = Arrays.binarySearch(arr, key);
``` 

- **Searching for the given key within the given range of the given array:**

```java
import java.util.Arrays;
int[] arr = {20, 10, 4, 11, 54};
int key = 11;
// Search in the range of indices between 1 and 3 after sorting
Arrays.sort(arr);
int index = Arrays.binarySearch(arr, 1, 3, key);
// if say the key is 54 then the output will be -4, where - indicates the key is not present in the 
// specified range. However; it is present in the array at an index 4.
// Now, let us say that the key is 12. It will return the -3, where - indicates the key is not 
// present in the array and if it were present then it would appear at index 3 of the array.
// Conclusion: Negative value represents the value is not present in the array.
```
**NOTE: To use this method always remember to _sort_ the given array before applying it.**

#### 4. Using fill() method:

`Arrays.fill()` method is used to fill the specified value to every index of the mentioned array. **One notable use of this method is to fill the boolean true value or 1 to each element of the array while applying sieve of Eratosthenes.**

```java
import java.util.Arrays;
boolean[] arr = new boolean[size];
Arrays.fill(arr, true);
```

#### 5. Using equals() method:
This method returns true if the given two arrays are equal.

```java
import java.util.Arrays;
int[] arr1 = {1, 2, 3, 4};
int[] arr2 = {1, 2, 3, 4};
int[] arr3 = {2, 1, 4, 3};
System.out.println(Arrays.equals(arr1, arr2)); // TRUE
System.out.println(Arrays.equals(arr1, arr3)); // FALSE
```
**NOTE: Two arrays are equal if they contain the same element in the same order.**

#### 6. Using copyOf() and copyOfRange() methods:
- `copyOf()` method copies the specified array, **truncating or padding with zeros(if necessary)** so the copy has the specified length.

```java
import java.util.Arrays;

int[] arr1 = new int[]{13, 24, 32};
int[] arr2 = Arrays.copyOf(arr1, 5); // Second argument is the length of the new array
// Content of arr2 = {13, 24, 32, 0, 0}
```

- Similarly, `copyOfRange(int[] original, int from, int to)` method specified range of the specified array into a new array. The final index of the range (to) must be greater than or equal to initial range (from). The final range if greater than the original.length; then **0s will be padded after** the `(original.length - 1)` indexes.

```java
import java.util.Arrays;
int[] arr1 = new int[]{13, 24, 32};
int[] arr2 = Arrays.copyOfRange(arr1, 1, 4);
// Array arr2 Content: {24, 32, 0, 0} 
```

There are other methods of Arrays class; but these are the most frequently used and required methods which are used in competitive coding. For more methods of the class please refer the [oracle docs.](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Arrays.html)
