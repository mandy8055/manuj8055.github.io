---
title: Collections and its key interfaces
tags: [Core Java, Collections]
style:
color:
description: A brief about Collections and its 9 key interfaces.
comments: true
---

<a class="text-center" href="https://feedburner.google.com/fb/a/mailverify?uri=Mandy8055&amp;loc=en_US" onclick="window.open(this.href, 'subscribe',
    'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;">Subscribe for New Posts</a>
    
---

# Collections and their requirement
##### Collections
If we want to represent a group of individual objects as a single entity then we should go for collection.
##### Collection Framework
It contains several classes and interfaces which can be used to represent a group of individual objects as a single entity.
##### Why Collections came to picture at the first place?
Initially, when we need to represent multiple homogenous values then we defined multiple variables. However, this practice was not desirable at all because it degraded the readability of code thereby deteriorating the norms of programming. Then we came up with the idea of **Arrays.** *An array is an indexed collection of fixed number of homogeneous data elements.* The main advantage of arrays is we can represent multiple homogenous values by using single variable which in turn improved the readability of the code. There were limitations with arrays too which eventually led to the requirement of **Collections.**

##### Difference between arrays and Collections

|**Arrays**|**Collections**|
| --- | --- |
| 1. Fixed in size.| 1. Growable in size |
| 2. With respect to performance related to memory; arrays are not recommended to use. | 2. With respect to performance related to memory; Collections are highly recommended to use. |
| 3. With respect to temporal performance; arrays are highly recommended to use. | 3. With respect to temporal performance; Collections are not [*favorable*](https://javarevisited.blogspot.com/2016/01/9-difference-between-array-vs-arraylist-in-java.html#axzz6mb6W7wUI) to be used. |
| 4. Arrays can hold only homogeneous data elements.  | 4. Collections can hold both homogeneous and heterogeneous data elements.                                                                                                                             |
| 5. Arrays can hold both primitives and objects.                                      | 5. Collections can hold only objects but not primitives.|

# 9 Key interfaces of Collections framework

- Collection
- List
- Set
- SortedSet
- NavigableSet
- Queue
- Map
- SortedMap
- NavigableMap

### Collection(I)
- If we want to represent a group of individual objects as a single entity, then we should go for Collection.
- Collection interfaces define the most common methods which are applicable for any collection object.
- In general, Collection interface is considered as **root** interface of collection framework.
- There is no concrete class which implements Collection interface directly.

##### Difference between Collection and Collections
Collection is an **interface.** If we want to represent a group of individual objects as a single entity then we should go for Collection.
Collections is a **utility class** present in <mark style="background-color: yellow">java.util package</mark> to define several utility methods for collection objects (like sorting, searching,etc).
### List
- It is the child interface of Collection. 
- If we want to represent a group of individual objects as a single entity where <mark style="background-color: yellow">duplicates are allowed and insertion order must be preserved</mark> then we should go for List.
### Set
- It is the child interface of Collection.
- If we want to represent a group of individual objects as a single entity where <mark style="background-color: yellow">duplicates are not allowed and insertion order not required to preserve</mark> then we go for Set interface.
### SortedSet
- It is the child interface of Set.
- If we want to represent a group of individual objects as a single entity where duplicates are not allowed and all objects should be inserted according to **some sorting order** then we should go for SortedSet.
### NavigableSet
1.	It is the child interface of SortedSet.
2.	It contains several methods for Navigation purposes.
3.	The implementation class for NavigableSet is _**TreeSet.**_

##### Differences between List and Set

|List|Set|
|---|---|
| 1. Duplicates are allowed. | 1. Duplicates are not allowed. |
| 2. Insertion order is preserved. | 2. Insertion order is not preserved. |

### Queue
- It is the child interface of Collection.
- If we want to represent a group of individual objects prior to processing then we should go for Queue.
- Usually Queue follows **First In First Out(FIFO)** order but based on our requirement we can implement our own priority order also.

Below image shows the gist of the above interfaces.

{% include elements/figure.html image="https://mandy8055.github.io/assets/2021-02-16-collections-1.png" caption="Collections-1" %}


**NOTE: All the above interfaces (Collection, List, Set, SortedSet, NavigableSet and Queue) meant for representing a group of individual objects.** 

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}