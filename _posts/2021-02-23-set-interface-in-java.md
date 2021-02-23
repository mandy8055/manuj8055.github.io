---
title: Set interface in Java
tags: [Core java, Collections]
description: Introduction to Set and various classes implementing Set interface in java Collections framework.
comments: true
mathjax: true
style:
color:
---

## Set

1. `Set` is the child interface of `Collection`.
2. If we want to represent a group of individual objects as a single entity where **duplicates** are not allowed and **insertion order** is not preserved.
3. `Set` interface doesn't contain any new method and we have to use only `Collection` interface methods.

## Implementation classes for Set interface

### 1. HashSet

- The underlying data structure is **hash table.**
- Insertion order is based on **`HashCode`** of objects.
- `null` insertion is possible (only once).
- Heterogenous objects are allowed.
- Implements `Serializable` and  `Cloneable` but not `RandomAccess` interfaces.
- `HashSet` is the best choice whenever frequent operation is **search operation.**

**NOTE: In `HashSet` duplicates are not allowed. If we are trying to insert duplicates, then we won't get any `Compile-time` or `Runtime` errors and add method simply returns false.**

```java
HashSet h = new HashSet();
System.out.println(h.add('A')); //returns true
System.out.println(h.add('A')); // returns false 
```

#### Constructors:

```java
HashSet h = new HashSet();
```
- The above definition creates an empty `HashSet` object with <mark style="background-color: azure">default initial capacity 16</mark> and default [fill ratio(or load factor)](https://www.geeksforgeeks.org/load-factor-and-rehashing/) of **0.75**.

```java
HashSet h = new HashSet(int initialCapacity);
```
- The above definition creates an empty `HashSet` object with specified `initialCapacity` and default fill-ratio.

```java
HashSet h = new HashSet(int initialCapacity, float fillRatio);
```

```java
HashSet h = new HashSet(Collection c);
```
- The above definition creates an equivalent `HashSet` for the given `Collection` c. This constructor is meant for inter-conversion between `Collection` objects.

**Note: Fill ratio or Load Factor:** After filling how much ratio, a new `HashSet` object will be created, this ratio is called *Fill ratio* or *load-factor.* For e.g. fill ratio 0.75 means after filling _75%_ ratio a new `HashSet` object will be created.

#### Implementation demo

```java
public static void main(String[] args){
    HashSet h = new HashSet();
    h.add("B");
    h.add("C");
    h.add("D");
    h.add("Z");
    h.add(null);
    h.add(10);
    System.out.println(h.add("Z")); // false
    System.out.println(h); // [Undefined order]
}
```

### 2. LinkedHashSet

- It is the child class of `HashSet`.
- It is same as `HashSet`(including `constructors` and `methods`) except the following differences:

| HashSet                                             | LinkedHashSet                                                                     |
| --------------------------------------------------- | --------------------------------------------------------------------------------- |
| 1. The underlying data structure is **hash table.** | The underlying data structure is a combination of **linked list and hash table.** |
| 2. Insertion order is **not preserved.**            | Insertion order is **preserved.**                                                 |
| 3. Introduced in java 1.2 version.                  | Introduced in 1.4 version.                                                        |

In the above `HashSet` implementation demo, if we replace `HashSet` with `LinkedHashSet`, then the output is **<code>[B, C, D, Z, null, 10]</code> i.e. insertion order is preserved.**

**Note: In general, we can use `LinkedHashSet` to develop [Cache based application](https://algorithms.tutorialhorizon.com/least-recently-used-lru-cache-using-linkedhashset-and-deque-set-2/).**

### 3. SortedSet

- `SortedSet` is the child interface of `Set`.
- If we want to represent a group of individual objects according to **some sorting logic** without duplicates then, we should go for `SortedSet`.
- `SortedSet` interface defines the following specific methods:

```java
// Returns first element of the SortedSet.
public Object first();
// Returns last element of the SortedSet.
public SortedSet last(Object obj);
// Returns SortedSet whose elements are less than obj
public SortedSet headSet(Object obj);
// Returns SortedSet whose elements are more than or equal to obj.
public SortedSet tailSet(Object obj);
// Returns SortedSet whose elements are greater than or equal to obj1 and less than obj2.
public SortedSet subSet(Object obj1, Object obj2);
// Returns Comparator object that describes underlying sorting technique. If we are using default natural sorting order then we will get null.
public Comparator comparator();
```

**Note: The default natural sorting order for numbers is ascending order and for `String` objects is alphabetical order.**

e.g.

Let the set has values:

$[100, 101, 104, 106, 110, 115, 120]$ 

```java
System.out.println(first()); // 100
System.out.println(last()); // 120
System.out.println(headSet(106)); // [100, 101, 104]
System.out.println(tailSet(106)); // [106, 110, 115, 120]
System.out.println(subSet(101, 115)); // [101, 104, 106, 110]
System.out.println(comparator()); // null since the given Set is in default natural order i.e. Ascending. 
```



{% include mathjax.html %}