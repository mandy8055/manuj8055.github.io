---
title: Cursors in java
tags: [Core java, Collections]
description: Introduction to Cursors and their types in java Collections framework.
comments: true
mathjax:
style:
color:
---

## Purpose of Cursor in Collections:

If we want to get objects **one by one** from the `Collection` then, we should go for `Cursor`. There are four types of `Cursors` available in java:
- Enumeration
- Iterator
- ListIterator
- Spliterator

**Note: `Spliterator` is introduced in java 8 so I'll discuss about it in some other post when I'll be brushing up my skills on java 8 feature.**

#### Enumeration

- We can use `Enumeration` to get objects one by one from legacy `Collection` object.
- We can create `Enumeration` objects by using <code>elements()</code> method of `Vector` class.

```java
public Enumeration elements();
// e.g.
Enumeration e = v.elements(); // where v is some legacy Collection object
```

##### Methods of Enumeration

```java
public boolean hasMoreElements(); // checks whether more elements are present in the Collection.
public Object nextElement(); // Returns the next element
```

#### Implementation Demo:

```java
Vector v = new Vector();
for(int i = 0; i < 11; i++){
    v.addElement(i);
    }
    System.out.println(v);
    Enumeration e = v.elements();
    while(e.hasMoreElements()){
        Integer i = (Integer) e.nextElement();
        if(i % 2 == 0)
            System.out.print(i + " ");
    }
```

#### Limitations of Enumeration

1. We can apply `Enumeration` concept only for **legacy classes** therefore, `Enumeration` is not a universal `Cursor`.
2. By using `Enumeration`, we can get only read access but we **cannot** perform **remove operation.**

To overcome above limitations, we should go for `Iterator`.

## Iterator

1. We can apply `Iterator` concept for any `Collection` object and hence it is **universal `Cursor`.**
2. By using `Iterator`, we can perform both _**read and remove operations.**_
3. We can create `Iterator` object by using `iterator()` method of `Collection` interface.

```java
public Iterator iterator();
e.g.
Iterator itr = c.iterator(); // where c is any Collection object.
```

#### Methods of Iterator

```java
public boolean hasNext();
public Object next();
public void remove();
```
**Note: You can see the methods for any Classes or interface using the command <code>javap java.util.Iterator</code>.**

#### Implementation Demo

```java
List al = new ArrayList(); // To remove warning use Generics
        for(int i = 0; i < 11; i++)
            al.add(i);
        Iterator itr = al.iterator();
        while(itr.hasNext()){
            Integer i = (Integer) itr.next();
            if(i % 2 != 0) // Remove all the odd numbers from ArrayList al
                itr.remove();
        }
        System.out.println(al); // [0, 2, 4, 6, 8, 10]
    }
```

#### Limitations of Iterator

1. By using `Enumeration` and `Iterator`, we can always iterate only in forward direction and we cannot iterate the `Collection` object in backward direction. These are **single direction Cursors.**
2. By using `Iterator`, we can perform only **read** and **remove** operations and we cannot perform **replacement and addition of new objects.**

To overcome above limitations, we should go for `ListIterator`.

## ListIterator

1. `ListIterator` is the child interface of `Iterator` and hence all methods present in `Iterator` are available to the `ListIterator` by default.
2. By using `ListIterator`, we can iterate the `Collection` objects either in forward or backward direction and hence it is **bi-directional cursor.**
3. By using `ListIterator`, we can perform **replacement and addition of new objects** in addition to read and remove operations.
4. We can create `ListIterator` by using `listIterator()` method of `List` interface.

```java
public ListIterator listIterator();
// e.g.
ListIterator ltr = l.listIterator(); // where l is any List object.
```

#### Methods of ListIterator

`ListIterator` defines the following 9 methods:

```java
// The below three methods are meant for forward iteration
public boolean hasNext();
public Object next();
public int nextIndex();
// The below three methods are meant for backward iteration
public boolean hasPrevious();
public Object previous();
public int previousIndex();
// Extra operations
public void remove();
public void add(Object o);
public void set(Object o);
```

**Note: You can see the methods for any Classes or interface using the command <code>javap java.util.ListIterator</code>.**

#### Implementation Demo

```java
LinkedList ll = new LinkedList();
ll.add("Mandy8055");
ll.add("David");
ll.add("Nag");
ll.add("Saurabh");
System.out.println(ll);
ListIterator ltr = ll.listIterator();
while(ltr.hasNext()){
    String s = ltr.next().toString();
    if(s.equals("Mandy8055"))
        ltr.remove();
    else if(s.equals("Saurabh"))
        ltr.add("Lipika");
    else if(s.equals("Nag"))
        ltr.set("Shweta");
    }
System.out.println(ll); // [David, Shweta, Saurabh, Lipika]
```

The most powerful `Cursor` is `ListIterator` but its limitation is it is applicable only for `List` interface.

#### Comparison(gist) between 3 Cursors:

| Properties                      | Enumeration                                  | Iterator                                           | ListIterator                              |
| ------------------------------- | -------------------------------------------- | -------------------------------------------------- | ----------------------------------------- |
| **1. where we can apply.**      | Only for legacy classes.                     | Applicable for any `Collection` classes.           | Applicable only for `List` Objects.       |
| **2. Is it legacy?**            | Yes                                          | NO                                                 | NO                                        |
| **3. Iteration Direction**      | Forward direction                            | Forward direction                                  | Bi-directional i.e. forward and backward. |
| **4. Allowed operations**       | Only read operation                          | Read and remove operation.                         | Read, remove, add and replace operation.  |
| **5. Method to get the Cursor** | Using `elements()` method of `Vector` class. | Using `iterator` method of `Collection` interface. | Using `ListIterator` of `List` interface. |
| 6. **Important Methods**        | [2 Methods](#methods-of-enumeration)         | [3 methods](#methods-of-iterator)                  | [9 methods](#methods-of-listiterator)     |

#### Internal Implementation of Cursors:
This is one of the most disturbing topic which I came through when I was preparing for my java certification. I was thinking every time we want to define a `Cursor` on some `Collection` object, I would create an object like `Enumeration e` or `Iterator itr`, etc. but then I realized how can we even think of creating an object for Interface:dizzy_face: since, `Enumeration`, `Iterator` and `ListIterator` are Interfaces.

Finally, I got the solution of this problem by running the below program.

```java
Vector v = new Vector();
Enumeration e = v.elements();
System.out.println(e.getClass().getName());
Iterator itr = v.iterator();
System.out.println(itr.getClass().getName());
ListIterator ltr = v.listIterator();
System.out.println(ltr.getClass().getName());
```

- The first print statement showed **<code>java.util.Vector$1</code>.** <mark style="background-color: yellow">The meaning of <code>$1</code> is there is an anonymous inner class of <code>Vector</code> class and the object is created for that anonymous inner class.</mark>
- Similarly, the second and third print statement showed **<code>java.util.Vector$Itr</code>.** and **<code>java.util.Vector$ListItr</code>.** <mark style="background-color: yellow">The meaning of $Itr and $ListItr is there are inner classes <code>Itr</code> and <code>ListItr</code> of <code>Vector</code> class simultaneously and the objects are created for those inner class.</mark> 

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}