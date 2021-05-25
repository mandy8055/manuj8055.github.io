---
title: Java Generics part 1
tags: [Java, Generics]
style: 
color: 
description: Why Java generics came to picture at the first place.
comments: true
---

## Introduction
The main objectives of using generics in java are to provide **type-safety** and to **resolve type-casting problems**.

### Type-Safety
`Arrays` are type-safe by default i.e. we can guarantee for the type of elements present inside array. For instance, if our programming requirement is to hold or store only `String` type of objects, we can choose `String` array. By mistake, if we are trying to add any other type of objects, we will get compile-time error.

```java
String[] arr = new String(100);
arr[0] = "Mike";
arr[1] = "Teflon";
arr[2] = new Integer(2); // Compile-time error
```

Hence, string array can contain only string type of objects. Because to this, we can guarantee for the **type-safety** of element present inside the array.

`Collection` objects are not **type-safe** i.e. we cannot guarantee for the type of elements present inside `Collection` object. For instance, if our programming requirement is to hold only `String` type of objects, and if we choose `ArrayList`, by mistake if we are trying to add any other type of object; we will not get any compile-time error but the program may fail at runtime.

```java
ArrayList al = new ArrayList(100);
al.add("Ravi");
al.add("John");
al.add(new Integer(3));
// Retrieval
String name1 = al.get(0);
String name2 = al.get(1);
String name3 = al.get(2); // Runtime exception: ClassCastException
```
Hence,  we cannot guarantee for the **type-safety** of elements present inside `Collections`.

### Type-Casting
In case of **array**, at the time of retrieval it is not required to perform **type-casting** because there is a guarantee for the type of elements present inside array.

```java
String[] arr = new String[100];
arr[0] = "Rhonda";
String name1 = arr[0]; // No type-casting required.
```

In the case of **collections**, at the time of retrieval we should perform **type-casting** compulsorily because there is no guarantee for the type of elements present inside collection.

```java
ArrayList al = new ArrayList();
al.add("Rhonda");
String name1 = al.get(0); // Compile-time error; incompatible types.
String name1 = (String) al.get(0); // Type-casting is mandatory. 
```
Hence, **type-casting** is a trade-off in collections. 

To overcome the above problems in **Collections** framework, `generics` concept was introduced in **_1.5 version_**.

## How to create a generic Collection objects

To hold only `String` type of objects, we can create `Generic` version of `ArrayList` as:

```java
ArrayList<String> al = new ArrayList<String>();
```

For this `ArrayList` we can add only `String` type of objects. If we are trying to add any other type by mistake then we will compile-time error.

```java
al.add("Cydney");
al.add(new Integer(23)); // Compile-time error.
```

Hence through generics, we are getting **type-safety**.

At the time of retrieval, we are not required to perform **type-casting**.

```java
ArrayList<String> al = new ArrayList<String>();
al.add("Mike");
String name1 = al.get(0); // Type-casting is not required.
```
Hence through generics, we can solve **type-casting** problem.

## Important Conclusions

1. **Polymorphism**(usage of parent reference to hold child object) concept is applicable only for the **base type** but not for **parameter type**.

```java
ArrayList<String> arrayList = new ArrayList<String>();
// ArrayList is the base type and String is the parameter type.
List<String> arrayList1 = new ArrayList<String>(); // allowed since base type can be polymorphic.
Collection<String> arrayList1 = new ArrayList<String>(); // allowed since base type can be polymorphic.
List<Object> arrayList2 = new ArrayList<String>(); // Compile-time error incompatible types.
```

2. For the **type-parameter**, we can provide any class or interface name but not primitives. If we are trying to provide primitives, then we will get compile-time error.

```java
ArrayList<int> al = new ArrayList<int>(); // CE: unexpected type, found: int required: reference.
```



{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}