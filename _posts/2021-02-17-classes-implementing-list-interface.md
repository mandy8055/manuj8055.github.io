---
title: Important classes implementing List interface in java
tags: [Core Java, Collections]
style:
color:
description: A brief note on key classes which implement List interface.
comments: true
---

<a class="text-center" href="https://feedburner.google.com/fb/a/mailverify?uri=Mandy8055&amp;loc=en_US" onclick="window.open(this.href, 'subscribe',
    'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;">Subscribe for New Posts</a>
        
---

## ArrayList

- The underlying data structure is **resizable array** or growable array.
- Duplicates are allowed.
- Insertion order is preserved.
- Heterogenous objects are allowed.*<mark>Except <code>TreeSet</code> and <code>TreeMap</code> heterogeneous objects are allowed everywhere inside <code>Collection</code>.</mark>*
- **Null insertion** is possible.

#### Constructors in ArrayList

```java
 ArrayList l = new ArrayList();
 ``` 
- The above initialization creates an empty <code>ArrayList</code> object with default <mark style="background-color: yellow">initial capacity 10.</mark>
- Once <code>ArrayList</code> reaches its max capacity; then new <code>ArrayList</code> object will be created with <mark style="background-color: yellow">new capacity = (currentCapacity * (3/2)) + 1.</mark> 
- The reference variable is reassigned to new object and the old <code>ArrayList</code> object is available for garbage collection.

```java
ArrayList l = new ArrayList(int initialCapacity);
```
- The above initialization creates an empty <code>ArrayList</code> object with specified initial capacity.

```java
ArrayList l = new ArrayList(Collection c);
```
- The above initialization creates an equivalent <code>ArrayList</code> object for the given **<code>Collection</code>**.

##### Demo Implementation:

```java
import java.util.ArrayList;
public class ArrayListDemo {
    public static void main(String[] args){
        ArrayList l = new ArrayList(); // Warning comes if we're not using generics. To remove use
        // ArrayList<String> l = new ArrayList<String>();
        l.add("A");
        l.add(10); // auto-boxing(primitive to object conversion) performed internally.
        l.add("A");
        l.add(null);
        System.out.println(l); // l is reference to ArrayList object. 
        // If we're trying to print any object reference internally toString() method will be invoked. 
        l.remove(2);
        System.out.println(l); // [A, 10, null]
        l.add(2, "M");
        l.add("N");
        System.out.println(l); // [A, 10, M, null, N]
    }
}
```

**Note 1: <code>ArrayList</code> and <code>Vector</code> classes <mark style="background-color: yellow">implements <code>RandomAccess</code> interface</mark>, so that any random element we can access with the same speed.**

**Note 2: <code>RandomAccess</code> interface is present in <code>java.util</code> package and it doesn't contain any methods. It is a [marker interface.](https://javarevisited.blogspot.com/2012/01/what-is-marker-interfaces-in-java-and.html), where required ability will be provided automatically by the JVM.**

##### Demo Implementation

```java
ArrayList l = new ArrayList();
LinkedList l2 = new LinkedList();
System.out.println(l instanceof Serializable); // true
System.out.println(l2 instanceof Serializable); // true
System.out.println(l instanceof Cloneable); // true
System.out.println(l2 instanceof Cloneable); // true
System.out.println(l instanceof RandomAccess); // true
System.out.println(l2 instanceof RandomAccess); // false
```

##### Pros and Cons of using ArrayList

- <code>ArrayList</code> is the best choice if frequent operation is retrieval operation because <code>ArrayList</code> implements <code>RandomAccess</code> interface.
- <code>ArrayList</code> is the worst choice if frequent operation is insertion or deletion in the middle.

##### Differences between ArrayList and Vector

| ArrayList                                                                                                                | Vector                                                                                                           |
| ------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| 1. Every method present in the <code>ArrayList</code> is non-synchronized.                                               | 1. Every method present in the <code>Vector</code> is synchronized.                                              |
| 2. At a time, multiple threads are allowed to operate on <code>ArrayList</code> object and hence it is not thread-safe.  | 2. At a time, only one thread is allowed to operate on <code>Vector</code> object and hence it is thread-safe.   |
| 3. Relatively, performance is high because threads are not required to wait to operate on <code>ArrayList</code> object. | 3. Relatively, performance is low because threads are required to wait to operate on <code>Vector</code> object. |
| 4. Introduced in 1.2v and it is non-legacy.                                                                              | 4. Introduced in 1.0v and it is legacy class.                                                                    |

##### How to get Synchronized version of ArrayList object

<code>ArrayList</code> is not synchronized by default. But we can get synchronized version of <code>ArrayList</code> object by using <code>synchronizedList()</code> method of <code>Collections</code> class.

```java
ArrayList l = new ArrayList();
List l1 =  Collections.synchronizedList(l); // l is not-synchronized while l1 is synchronized.
```

**Note: We can get synchronized version of <code>Set</code> and <code>Map</code> object by using the following methods of <code>Collections</code> class i.e. <code>synchronizedSet()</code> and <code>synchronizedMap()</code> methods.**

## LinkedList

- The underlying data-structure is **doubly-linked list**.
- Insertion order is preserved.
- Duplicate objects are allowed.
- Heterogeneous objects are allowed.
- **Null insertion** is possible.
- <code>LinkedList</code> implements <code>Serializable</code> and <code>Cloneable</code> but not <code>RandomAccess</code> interface.
- <code>LinkedList</code> is the best choice if frequent operation is insertion or deletion in the middle.
- <code>LinkedList</code> is the worst choice if frequent operation is retrieval operation.

#### Constructors in LinkedList

```java
LinkedList l = new LinkedList();
```
- The above initialization creates an empty <code>LinkedList</code> object.

```java
LinkedList l = new LinkedList(Collection c);
```
- The above initialization creates an equivalent <code>LinkedList</code> object for the given <code>Collection</code>.

#### Specific methods of LinkedList class

- Usually, we can use <code>LinkedList</code> to develop stacks and queues. To provide support for this requirement <code>LinkedList</code> class defines specific methods.

```java
void addFirst(Object o);
void addLast(Object o);
Object getFirst();
Object getLast();
Object removeFirst();
Object removeLast();
```

#### Demo Implementation

```java
import java.util.LinkedList;
public class LinkedListDemo {
    public static void main(String[] args) {
        LinkedList l = new LinkedList();
        l.add("Mandy8055");
        l.add(30);
        l.add(null);
        l.add("Mandy8055"); // [Mandy8055, 30, null, Mandy8055]
        l.set(0, "Software"); // [Software, 30, null, Mandy8055]
        l.add(0, "Sappy"); // [Sappy, Software, 30, null, Mandy8055]
        l.removeLast(); // [Sappy, Software, 30, null]
        l.addFirst("CCC"); // [CCC,Sappy, Software, 30, null]
        System.out.println(l);
    }
}
```

## Vector

- The underlying data structure is **resizable/growable array.**
- Insertion order is preserved.
- Duplicates are allowed.
- Heterogeneous objects are allowed.
- **Null insertion** is possible.
- It implements <code>Serializable</code>, <code>Cloneable</code> and <code>RandomAccess interfaces</code>.
- Every method present in the <code>Vector</code> is _**synchronized**_ and hence <code>Vector</code> object is [thread-safe](https://dzone.com/articles/what-does-thread-safety-mean-in-java#:~:text=Conclusion-,Thread%20safety%20in%20Java%20means%20that%20the%20methods%20of%20a,we%20call%20the%20quiescent%20method.).

#### Constructors

```java
Vector v = new Vector();
```
- The above initialization creates an empty <code>Vector</code> object with <mark style="background-color: azure">default initial capacity 10.</mark>
- Once vector reaches its, max-capacity, then a new Vector object will be created with **<mark style="background-color: yellow">new_capacity = current_capacity * 2.</mark>**

```java
Vector v = new Vector(int initialCapacity);
```
- The above initialization creates an empty <code>Vector</code> object with specified initial capacity.

```java
Vector v = new Vector(int initialCapacity, int incrementalCapacity);
```

```java
Vector v = new Vector(Collection c);
```
- The above initialization creates an equivalent <code>Vector</code> object for the given <code>Collection</code> c. This constructor is meant for inter-conversion between <code>Collection</code> objects.

#### Vector-specific methods

```java
void addElement(Object o);
boolean removeElement(Object o);
void removeElementAt(int index);
void removeAllElements();
int size();
int capacity();
Enumeration elements();
```

#### Demo Implementation

```java
import java.util.*;
public class VectorDemo1 {
    public static void main(String[] args){
        Vector v = new Vector(); // Warning comes because of lack of type safety. Use generics
//        Vector v = new Vector(10, 5);
//        Vector v = new Vector(24);
        System.out.println(v.capacity());
        for(int i = 1; i <= 10; i++){
            v.addElement(i); // auto-boxing will be performed automatically.
        }
        System.out.println(v.capacity());
        v.add("A");
        System.out.println(v.capacity());
        System.out.println(v);
    }
}
```

#### Stack

- It is the child class of <code>Vector</code>.
- It is a specially designed class for **Last In First Out(LIFO)** order.

##### Stack-specific methods

```java
Object push(Object o);
Object pop();
Object peek();
boolean empty();
int search(Object o); // returns the offset of o if o is present inside the stack else returns -1. 
```

##### Implementation Demo

```java
import java.util.Stack;
public class StackDemo {
    public static void main(String[] args){
        Stack s = new Stack();
        s.push("A");
        s.push("B");
        s.push("C");
        System.out.println(s);
        System.out.println(s.search("A"));
        System.out.println(s.search("Z"));
    }
}
```
{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}