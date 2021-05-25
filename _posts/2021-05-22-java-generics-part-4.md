---
title: Java Generics part 4
tags: [Java, Generics]
style: 
color: 
description: Understanding generic methods and wildcard character.
comments: true
---

In previous posts, we saw the basic introduction to **generics** and we also understood how generics is implemented internally and got a glimpse about **bounded-types** and their syntaxes. In this post we'll try to understand about **generic methods** and **wildcard character(?)**.

## Generic Methods and wildcard character(?)

1. **<code>m1(ArrayList<String> l)</code>:** 
   1. We can call this method by passing `ArrayList` type `String` only.
   2. Within the body above method, we can only add `String` type of objects to the `ArrayList`. If we are trying to add any other type by mistake, then we will get compile-time error.

```java
m1(ArrayList<String> l){
    l.add("Ottoman"); // Valid
    l.add(null); // valid
    l.add(10); // Compile-time error
}
```
2. **<code>m1(ArrayList<?> l)</code>:**
   1. We can call this method by passing `ArrayList` of **any** type.
   2. Within the body of above method, we cannot add anything to the list except `null` because we do not know the type exactly.
   3. `null` is allowed because it is valid value for any type.
   4. These type of methods are best suitable for **read-only** operations.

```java
m1(ArrayList<?> l){
    l.add(10.5); // Invalid
    l.add("A"); // Invalid
    l.add(10); // Invalid
    l.add(null); // VALID
}
``` 
3. **<code>m1(ArrayList<? extends X> l)</code>:**
   1. `X` can be either class or interface.
   2. If `X` is a class, then we can call this method by passing `ArrayList` of either `X` type or its **child classes**.
   3. If `X` is an `interface`, then we can call this method by passing `ArrayList` of either `X` type or its **implementation classes.**
   4. Within the body of above method, we cannot add anything to the list except `null` because we do not know the type of `X` or its child classes exactly.
   5. These type of methods are also best suitable for **read-only** operations.
   
4. **<code>m1(ArrayList<? super X> l)</code>:**
   1. `X` can be either class or interface.
   2. If `X` is a class, then we can call this method by passing `ArrayList` of either `X` type or its **super classes.**
   3. If `X` is an interface, then we can call this method by passing `ArrayList` of either `X` type or **super class of implementation class of `X`**.
   4. Within the body of this method, we can add **`X` type of objects** and `null` to the list.

### Some common implementations:
```java
ArrayList<String> l = new ArrayList<String>(); // VALID
ArrayList<?> l = new ArrayList<String>(); // VALID
ArrayList<?> l = new ArrayList<Integer>(); // VALID
ArrayList<? extends Number> l = new ArrayList<Integer>(); // VALID
ArrayList<? extends Number> l = new ArrayList<String>(); //INVALID
ArrayList<? super String> l = new ArrayList<Object>(); // VALID
ArrayList<?> l = new ArrayList<?>(); // CE: unexpected type required class or interface **without bounds.**
ArrayList<?> l = new ArrayList<? extends Number>(); // CE: unexpected type required class or interface **without bounds.**
```

### Declaring Type-parameter at Class level

```java
class Test<T>{
   // We can use `T` within this class based on our requirement.
}
```
### Declaring Type-parameter at method level
We have to declare **type-parameter** just before **return type**.
```java
class Test{

   public <T> void m1(T ob){
      // We can use T anywhere within this method based on our requirement. 
   }
}
```
We can define **bounded types** even at method level also.
```java
public <T> void m1(T ob);
public <T extends Runnable> void m1(T ob);
public <T extends Number> void m1(T ob);
public <T super String> void m1(T ob);
public <T extends Runnable & Comparable> void m1(T ob);
public <T extends Number & Runnable> void m1(T ob);
```

## Communication with Non-generic code
If we send **generic object** to **non-generic** area then, it starts behaving like non-generic object. Similarly, if we send **non-generic object** to **generic** area then, it starts behaving like generic object i.e. the location in which object is present behavior will be defined accordingly. e.g.

```java
class Test{
   public static void main(String[] args){
      // generic area
      ArrayList<String> al = new ArrayList<String>();
      al.add("Bose");
      al.add("Jim");
      al.add(new Integer(10)); // CE
      m1(al);
      System.out.println(al); // [Bose, Jim, 10.5, 10, true]
      al.add(10.5); // Compile-time error
   }
   // Non-generic area
   public static void m1(ArrayList al){
      al.add(10.5);
      al.add(new Integer(10));
      al.add(true);
   }
}
```
## Conclusions
1. The main purpose of generics is to provide type-safety and to resolve type-casting problems.
2. Type-safety and type-casting both are applicable at compile-time, hence generics concept is also applicable only at compile-time but not at runtime.
3. At the time of compilation, as the last step generics syntax will be removed and hence for the JVM generic syntax will not be available. Hence, the following declarations are equal.
```java
ArrayList al = new ArrayList<String>();
ArrayList al = new ArrayList<Integer>();
ArrayList al = new ArrayList<Double>();
ArrayList al = new ArrayList();
```
e.g.
```java
ArrayList al = new ArrayList<String>();
al.add(10);
al.add(10.5);
al.add(true);
System.out.println(al); // [10, 10.5, true]
```
4. The following declarations are equal. For these `ArrayList` objects, we can add only `String` type.
```java
ArrayList<String> al = new ArrayList<String>();
ArrayList<String> al = new ArrayList<>();
```
5. At compile-time, generally the code is compiled considering the **generic syntax** and then the compiler removes the generic syntax and after removing it **compiles the code again**. This is the reason the below code generates compile time error.
```java
class Test{
   public void m1(ArrayList<Integer> al) // ==> converts to m1(ArrayList al)
   {
      // body
   }
   public void m1(ArrayList<String> al) // ==> converts to m1(ArrayList al)
   {
      // body
   }
   // Compile-time error: name clash: Both methods having same erasure.
}
```



{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}