---
title: Java Generics part 3
tags: [Java, Generics]
style: 
color: 
description: Bounded Types and its Syntax.
comments: true
---

In the previous post, we saw the basic introduction to **generics** and we also understood how generics is implemented internally and look on some examples too. In this post we'll try to understand about **bounded-types** and **generic methods**.

## Bounded Types

We can bound the type-parameter for a particular range by using `extends` keyword. Such types are called **bounded-types**. For instance in the below example; as a type-parameter we can pass any type and there are no restrictions and hence it is **unbounded-type**.


```java
class Test<T>{
    // Class body
}
// For creating instance of Test
Test<Integer> t1 = new Test<Integer>(); // Valid
Test<String> t2 = new Test<String>(); // Valid
```

Now, let us see the **bounded-type** syntax and understand their meanings.

### Syntax for bounded-type

```java
class Test<T extends X>{
    // Class body
}
```

- X can be either **class** or **interface**.
- If X is a class, then as the type-parameter we can pass either **X type or its child classes**.
- If X is an interface, then as the type-parameter we can pass either **X type or its implementation classes**.
- Examples

**Example 1:**
```java
class Test<T extends Number>{
    // class Body
}
Test<Integer> t1 = new Test<Integer>(); // Valid
Test<String> t2 = new Test<String>(); // CE: type parameter java.lang.String is not in its bound.
```

**Example 2:**
```java
class Test<T extends Runnable>{
    // class Body
}
Test<Runnable> t1 = new Test<Runnable>(); // Valid
Test<Thread> t2 = new Test<Thread>(); // Valid Implementation class of Runnable interface.
Test<Integer> t3 = new Test<Integer>(); // CE: type parameter java.lang.Integer is not in its bound.
```
- We can define bounded-types even in combination also but with some restrictions. For example,

```java
// As the type parameter we can take anything which should be child class of Number and should implement Runnable interface. VALID
class Test<T extends Number & Runnable>{
    // class body
}
// As the type parameter we can take anything which should be child class of Number and should implement Runnable and comparable interface. VALID
class Test1<T extends Number & Runnable & Comparable>{
    // class body
}
// The below syntax is INVALID since we have to take class name first followed by interface name.
class Test2<T extends Runnable & Number>{
    // class body
}
// the below syntax is INVALID because java doesn't support multiple inheritance.
class Test3<T extends Thread & Number>{
    // class body
}
```

### Important conclusions:

1. We can define **bounded-types** only by using `extends` keyword and we cannot use `implements` or `super` keywords. We can instead replace `implements` keyword purpose with `extends` keyword.

```java
// VALID
class Test<T extends Number>{
    // class Body
}
// VALID
class Test<T extends Runnable>{
    // class Body
}
// INVALID
class Test<T implements Runnable>{
    // class Body
}
// INVALID
class Test<T super String>{
    // class Body
}
```
2. As the **type-parameter** `T`, we can take any valid java identifier name but it is convention to use `T`.
3. Based on our requirement, we can declare any number of **type-parameters** and all these **type-parameters** should be separated with comma(,).
```java
class Test<A, B>{
    // class body
}
class Test<A, B, C>{
    // class body
}
class HashMap<K, V>{
    // class body
}
```
{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}