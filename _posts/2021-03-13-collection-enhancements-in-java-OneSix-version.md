---
title: 1.6 Enhancements in Collection framework
tags: [Core Java, Collections]
style:
color:
description: A note on the enhancements which took place in java 1.6 Collection framework. 
comments: true
---

As a part of 1.6 version, the following two enhancements were introduced in Collection framework:
1. `NavigableSet`
2. `NavigableMap`

## NavigableSet
It is the child interface of `SortedSet` and it defines several methods for navigation purposes.

### Methods in NavigableSet interface
```java
floor(e); // Returns highest element which is <= e.
lower(e); // Returns highest element which is < e.
ceiling(e); // Returns highest element which is >= e.
higher(e); // Returns highest element which is > e.
pollFirst(); // Removes and returns the first element.
pollLast(); // Removes and returns the last element.
descendingSet(); // Returns NavigableSet in reverse order.
```

### Demo implementation
```java
import java.util.TreeSet;
public class NavigableSetDemo {
    public static void main(String[] args){
        TreeSet<Integer> treeSet = new TreeSet<Integer>();
        treeSet.add(1000);
        treeSet.add(2000);
        treeSet.add(3000);
        treeSet.add(4000);
        treeSet.add(5000);
        System.out.println(treeSet); // [1000, 2000, 3000, 4000, 5000]
        System.out.println(treeSet.floor(3000)); // 3000
        System.out.println(treeSet.lower(3000)); // 2000
        System.out.println(treeSet.ceiling(3000)); // 3000
        System.out.println(treeSet.higher(3000)); // 4000
        System.out.println(treeSet.pollFirst()); // 1000
        System.out.println(treeSet.pollLast()); // 5000
        System.out.println(treeSet.descendingSet()); // [4000, 3000, 2000]
        System.out.println(treeSet); // [2000, 3000, 4000]
    }
}
```

## NavigableMap
It is the child interface of `SortedMap`. It defines several methods for navigation purposes.

### Methods in NavigableMap interface
```java
floorKey(e); // Returns highest key which is <= e.
lowerKey(e); // Returns highest key which is < e.
ceilingKey(e); // Returns highest key which is >= e.
higherKey(e); // Returns highest key which is > e.
pollFirstEntry(); // Removes and returns the first entry.
pollLastEntry(); // Removes and returns the last entry.
descendingMap(); // Returns NavigableMap in reverse order.
```

### Demo Implementation
```java
import java.util.TreeMap;
public class NavigationMapDemo {
    public static void main(String[] args){
        TreeMap<String, String> treeMap = new TreeMap<String, String>();
        treeMap.put("A", "Apple");
        treeMap.put("B", "Ball");
        treeMap.put("C", "Cat");
        treeMap.put("D", "Dog");
        treeMap.put("E", "Elephant");
        treeMap.put("F", "Fan");
        System.out.println(treeMap);// {A=Apple, B=Ball, C=Cat, D=Dog, E=Elephant, F=Fan}
        System.out.println(treeMap.floorKey("C")); // C
        System.out.println(treeMap.lowerKey("C")); // B
        System.out.println(treeMap.ceilingKey("C")); // C
        System.out.println(treeMap.higherKey("C")); // D
        System.out.println(treeMap.pollFirstEntry()); // A: Apple
        System.out.println(treeMap.pollLastEntry()); // F: Fan
        System.out.println(treeMap.descendingMap()); // {E=Elephant, D=Dog, C=Cat, B=Ball}
        System.out.println(treeMap); // {B=Ball, C=Cat, D=Dog, E=Elephant}
    }
}
```
{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}