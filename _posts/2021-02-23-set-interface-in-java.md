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
public Object last();
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

### 4. TreeSet

- The underlying data structure is **Balanced tree.**
- Duplicate objects are not allowed.
- Insertion order is not preserved.
- Heterogeneous objects are **not allowed. If we are using heterogeneous objects, we will get `ClassCastException`.**
- `null` insertion is possible only once.
- `TreeSet` implements `Serializable` and `Cloneable` but not `RandomAccess` interfaces.
- All the objects will be inserted based on some sorting order. It may be default natural sorting order or customized sorting order.

#### Constructors

##### 1.
```java
TreeSet t = new TreeSet();
```
- The above definition creates an empty `TreeSet` object where the elements will be inserted according to default natural sorting order.

##### 2.
```java
TreeSet t = new TreeSet(Comparator c);
```
- The above definition creates an empty `TreeSet` object where the elements will be inserted according to customized sorting order specified by comparator object `c`.

##### 3.
```java
TreeSet t = new TreeSet(Collection c);
```

#### 4.
```java
TreeSet t = new TreeSet(SortedSet s);
```

#### Implementation demo 1:

```java
public static void main(String[] args){
        TreeSet treeSet = new TreeSet();
        treeSet.add("A");
        treeSet.add("a");
        treeSet.add("B");
        treeSet.add("Z");
        treeSet.add("L");
        // treeSet.add(10); --> results in ClassCastException
        // treeSet.add(null);
        System.out.println(treeSet); // [A, B, L, Z, a]
    }
```

##### Null Acceptance in TreeSet
- For **non-empty** `TreeSet` if we are trying to insert `null`, then we will get **`NullPointerException`.**
- For **empty** `TreeSet` as the first element `null` is allowed but after inserting that `null` if we are trying to insert any other element, then we will get the runtime exception i.e. `NullPointerException`.

**NOTE: Till java _1.6 version_, `null` is allowed as the first element to the empty `TreeSet` but from _1.7 version_ onwards `null` is not allowed even as the first element i.e. '`null` is not applicable for `TreeSet` from _1.7 version_ onwards'.**

#### Implementation demo 2:

```java
public static void main(String[] args){
    TreeSet treeSet = new TreeSet();
    treeSet.add(new StringBuffer("A"));
    treeSet.add(new StringBuffer("L"));
    treeSet.add(new StringBuffer("Z"));
    treeSet.add(new StringBuffer("B"));
    System.out.println(treeSet); // --> Gives ClassCastException
}
```

- If we are depending on default natural sorting order, the objects should be homogeneous and **comparable** compulsorily otherwise, we will get runtime exception i.e. `ClassCastException`.
- An object is said to be **comparable** if and only if corresponding class implements [java.lang.Comparable](https://docs.oracle.com/javase/7/docs/api/java/lang/Comparable.html) interface.
- `String` class and all **wrapper** classes already implement `Comparable` interface but `StringBuffer` and `StringBuilder` classes doesn't implement `Comparable` interface. That is the reason we got `ClassCastException` in the above demonstration.
- If we are depending on default natural sorting order then while adding objects into the  `TreeSet`, **JVM** calls `compareTo()` method.

**Note: If default natural sorting order is not available then we can go for customized sorting by using `Comparator` interface i.e. <mark style="background-color: yellow"><code>Comparable</code> meant for default natural sorting order whereas <code>Comparator</code> meant for customized sorting order.</mark>**

#### Comparison table of Set implemented classes

| Properties                        | HashSet        | LinkedHashSet              | TreeSet                                                          |
| --------------------------------- | -------------- | -------------------------- | ---------------------------------------------------------------- |
| **1. Underlying data structures** | Hash table     | Linked list and Hash table | Balanced binary tree.                                            |
| **2. Duplicate objects**          | Not allowed    | Not allowed                | Not allowed                                                      |
| **3. Insertion order**            | Not preserved  | Preserved                  | Not preserved                                                    |
| **4. Sorting order**              | Not Applicable | Not Applicable             | Applicable                                                       |
| **5. Heterogenous objects**       | Allowed        | Allowed                    | Not allowed. Special cases allowed when we use `Comparator`      |
| **6. `null` acceptance**          | Allowed        | Allowed                    | Allowed only in particular [cases](#null-acceptance-in-treeset). |

In the next blog post I'll share my learning on `Comparator` interface and its usage.

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}
{% include mathjax.html %}