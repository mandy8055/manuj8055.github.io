---
title: Queue interface in collections framework
tags: [Core Java, Collections]
style:
color:
description: A brief note Queue interface and its classes in java.
comments: true
---

## java 1.5 version enhancements(Queue interface)
- It is the child interface of `Collection`.
- If we want to represent a group of individual objects prior to processing, then we should go for queue.
- Usually, `Queue` follows **first in first out** order but based on our requirement we can implement our own priority order also(**priority queue**).
- From java 1.5 version onwards `LinkedList` class also implements `Queue` interface.
- `LinkedList` based implementation of `Queue` always follows **first in first out order**.

### Queue interface specific methods
```java
boolean offer(Object o); // --> Same as enqueue
// For seeing the first element.
Object peek(); // to return head(first) element of the queue. If queue is empty, this method returns null.
Object element(); // to return head(first) element of the queue. If queue is empty, this method raises RE: NoSuchElementException.
// For dequeue purposes
Object poll(); // to remove and return head element of the queue. If queue is empty then this method returns null.
Object remove(); // to remove and return head element of the queue. If queue is empty then this method raises RE: NoSuchElementException.
```

## Priority Queue:
- If we want to represent a group of individual objects prior to processing **according to some priority** then, we should go for `PriorityQueue`.
- The priority can be either default natural sorting order or customized sorting order defined by `Comparator`.
- Insertion order is not preserved and it is based on some priority.
- Duplicates are allowed.
- If we are depending on default natural order, the objects should be homogeneous and comparable compulsorily otherwise we will get `ClassCastException`.
- If we are defining our own sorting logic by using `Comparator` then, objects need not be homogenous and comparable.
- `null` is not allowed even as the head element also.

### Constructors

#### 1. 

```java
 PriorityQueue q = new PriorityQueue();
 ``` 
- The above initialization creates an empty <code>PriorityQueue</code> object with default <mark style="background-color: yellow">initial capacity 11.</mark> and all objects will be inserted according to default natural sorting order.

#### 2.
```java
PriorityQueue q = new PriorityQueue(int initialCapacity);
```
- The above initialization creates an empty <code>PriorityQueue</code> object with specified initial capacity.

#### 3. 
```java
PriorityQueue q = new PriorityQueue(int initialCapacity, Comparator c);
```

#### 4.
```java
PriorityQueue q = new PriorityQueue(Collection c);
```
- The above initialization creates an equivalent <code>PriorityQueue</code> object for the given **<code>Collection</code>**.

#### 5.
```java
PriorityQueue q = new PriorityQueue(SortedSet s);
```

### Implementation demo 1

```java
import java.util.PriorityQueue;
public class PriorityQueueDemo {
    public static void main(String[] args){
        PriorityQueue priorityQueue = new PriorityQueue();
//        System.out.println(priorityQueue.peek());
//        System.out.println(priorityQueue.element());
        for(int i = 0; i <= 10; i++)priorityQueue.offer(i);
        System.out.println(priorityQueue);
        System.out.println(priorityQueue.poll()); // 0
        priorityQueue.offer(1);
        System.out.println(priorityQueue); // [1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    }
}
```
**Important Note: Some platforms won't provide proper support for thread priorities and `PriorityQueue` classes. That is the reason we may get non-sorted outputs.** 

### Implementation demo 2
```java
import java.util.Comparator;
import java.util.PriorityQueue;
public class PriorityQueueDemo1 {
    public static void main(String[] args){
        PriorityQueue priorityQueue = new PriorityQueue(6, new MyComparator8());
        priorityQueue.offer("L");
        priorityQueue.offer("B");
        priorityQueue.offer("Z");
        priorityQueue.offer("A");
        System.out.println(priorityQueue); // [Z, L, B, A]

    }
}
class MyComparator8 implements Comparator{
    @Override
    public int compare(Object o1, Object o2) {
        String s1 = o1.toString();
        String s2 = o2.toString();
        return s2.compareTo(s1);
    }
}
```
There is a support for **double ended queues** in java which is provided by `Dequeue` interface. For further reading please visit [**this**](https://www.geeksforgeeks.org/deque-interface-java-example/) link.

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}