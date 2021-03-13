---
title: Utility classes for Collections framework
tags: [Core Java, Collections]
style:
color:
description: A brief note on Arrays and Collections utility classes.
comments: true
---

## Collections
- `Collections` class defines several utility methods for `Collection` objects like sorting, searching, reversing, etc.

### Sorting Elements of List
`Collections` class defines the following two sort methods:

#### 1.
```java
public static void sort(List list);
```
- To sort based default natural sorting order.
- In this case, `List list` should contain homogeneous and comparable objects compulsorily otherwise we will get `ClassCastException`.
- `List list` should not contain `null` otherwise we will get `NullPointerException`.

Implementation demo can be found below:
```java
import java.util.ArrayList;
import java.util.Collections;
public class CollectionsSortDemo {
    public static void main(String[] args){
        ArrayList arrayList = new ArrayList();
        arrayList.add("Z");
        arrayList.add("A");
        arrayList.add("K");
        arrayList.add("N");
        System.out.println("Before sorting " + arrayList); // Before sorting [Z, A, K, N]
        Collections.sort(arrayList);
        System.out.println("After sorting " + arrayList); // After sorting [A, K, N, Z]
    }
}
```

#### 2.
```java
public static void sort(List list, Comparator comparator);
```
- To sort the `List list` based on customized sorting order.

Implementation demo can be found below:
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
public class CollectionsSortDemo2 {
    public static void main(String[] args){
        ArrayList arrayList = new ArrayList();
        arrayList.add("Z");
        arrayList.add("A");
        arrayList.add("K");
        arrayList.add("N");
        System.out.println("Before sorting " + arrayList); // Before sorting [Z, A, K, N]
        Collections.sort(arrayList, new MyComparator7());
        System.out.println("After sorting " + arrayList); // After sorting [Z, N, K, A]
    }
}
class MyComparator7 implements Comparator{
    @Override
    public int compare(Object o1, Object o2) {
        String s1 = o1.toString();
        String s2 = o2.toString();
        return s2.compareTo(s1);
    }
}
```

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}