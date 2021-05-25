---
title: Java Generics part 2
tags: [Java, Generics]
style: 
color: 
description: Internal implementations of generics.
comments: true
---

In the previous post, we saw the basic introduction to **generics**. In this post we will dive deeper into generics concept and try to cement our understanding how generics is implemented internally.

## Generic Classes
Until *1.4 version*, a non-generic version of `ArrayList` class is declared as:

```java
// Till 1.4 version
class ArrayList{
    // Other methods
    add(Object o){
        // Body
    }
    Object get(int index){
        // Body of method
    }
}
```
The argument to `add()` is `Object` and therefore we can add any type of object to the `ArrayList`. Because of this we are missing **type-safety.**

The return type of `get()` is `Object` and therefore at the time of retrieval we have to perform **type-casting**.

In *1.5 version*, a generic version of `ArrayList` class is declared as:

```java
// Here T is type parameter
class ArrayList<T>{
    add(T t){
        // Body
    }
    T get(int index){
        // Body
    } 
}
```

**NOTE:** **Based on our runtime requirement, `T` will be replaced with our provided type.**

To hold only String type of Object a generic version of ArrayList object can be created as:

```java
ArrayList<String> al = new ArrayList<String>();
```
For this requirement, compiler-considered version of `ArrayList` class is as:

```java
class ArrayList<String>{
    add(String t){
        // Body
    }
    String get(int index){
        // Body
    } 
}
```

The argument to `add()` method is `String` type and therefore, we can add only `String` type of objects. If we trying to add any other type by mistake, we will get compile-time error.

```java
al.add("Jim");
al.add(new Integer(10)); // CE: Cannot find symbol add(java.lang.Integer) location: class ArrayList<String>
```

- In **generics**, we are associating a *type parameter* to the class. Such type of parameterized classes are nothing but **Generic classes** or **template classes**.
- Based on our requirement, we can define our own **generic classes** also. e.g.
```java
public class CustomGenericsDemo<T> {
    T object;
    public CustomGenericsDemo(T object){
        this.object = object;
    }
    public void show(){
        System.out.println("The object is of type: " + object.getClass().getName());
    }
    public T getObj(){
        return this.object;
    }
}
class Main{
    public static void main(String[] args){
        CustomGenericsDemo<Integer> customGenericsDemo = new CustomGenericsDemo<>(10); 
        customGenericsDemo.show(); // The object is of type: java.lang.Integer
        CustomGenericsDemo<String> customGenericsDemo1 = new CustomGenericsDemo<>("Cydney");
        customGenericsDemo1.show(); // The object is of type: java.lang.String
        CustomGenericsDemo<Double> customGenericsDemo2 = new CustomGenericsDemo<>(10.5);
        customGenericsDemo2.show(); // The object is of type: java.lang.Double
    }
}
```

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}