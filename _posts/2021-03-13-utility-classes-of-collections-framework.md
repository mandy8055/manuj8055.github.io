---
title: Utility classes for Collections framework
tags: [Core Java, Collections]
style:
color:
mathjax: true
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

### Searching elements of a list
`Collections` class defines the following methods:

#### 1.
```java
public static int binarySearch(List list, Object target);
```
- If the `list` is sorted according to default natural sorting order then, we use the above mentioned method.

#### 2.
```java
public static int binarySearch(List list, Object target, Comparator comparator);
```
- We have to use the above mentioned method if the list is sorted according to customized sorting order.

#### Conclusions
1. The above search methods use **binary search** algorithm internally.
2. Successful search returns **index**.
3. Unsuccessful search returns **insertion point.**
4. **Insertion point** is the location where we can place target element in the sorted `list`.
5. Before calling binary search method, the `list` should be sorted compulsorily otherwise, we will get unpredictable results.
6. If the `list` is sorted according to `comparator` then, at the time of search operation also we have to pass same `Comparator` object otherwise we will get unpredictable results.

#### Implementation demo 1
```java
import java.util.ArrayList;
import java.util.Collections;
public class CollectionsSearchDemo1 {
    public static void main(String[] args){
        ArrayList<String> arrayList = new ArrayList();
        arrayList.add("Z");
        arrayList.add("A");
        arrayList.add("M");
        arrayList.add("K");
        arrayList.add("a");
        System.out.println(arrayList); // [Z, A, M, K, a]
        Collections.sort(arrayList);
        System.out.println(arrayList); // [A, K, M, Z, a]
        System.out.println(Collections.binarySearch(arrayList, "M")); // 2
        System.out.println(Collections.binarySearch(arrayList, "b")); // -6(insertion point)
    }
}
```

#### Implementation demo 2

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
public class CollectionsSearchDemo2 {
    public static void main(String[] args){
        ArrayList<Integer> arrayList = new ArrayList<>();
        arrayList.add(15);
        arrayList.add(0);
        arrayList.add(20);
        arrayList.add(10);
        arrayList.add(5);
        System.out.println(arrayList); // [15, 0, 20, 10, 5]
        Collections.sort(arrayList, new MyComparator());
        System.out.println(arrayList); // [20, 15, 10, 5, 0]
        System.out.println(Collections.binarySearch(arrayList, 10,new MyComparator())); // 2
        System.out.println(Collections.binarySearch(arrayList, 13,new MyComparator())); // -3
        System.out.println(Collections.binarySearch(arrayList, 17)); // unpredictable
    }
}
class MyComparator implements Comparator{
    @Override
    public int compare(Object o1, Object o2) {
        Integer i1 = (Integer) o1;
        Integer i2 = (Integer) o2;
        return i2.compareTo(i1);
    }
}
```

**NOTE:** For the `list` of n elements, in the case of `binarySearch()` method:
1. Successful search result range : $0\hspace{0.2cm}to\hspace{0.2cm}(n-1)$
2. Unsuccessful result range : $-(n+1)\hspace{0.2cm}to\hspace{0.2cm}-1$
3. Total result range : $-(n+1)\hspace{0.2cm}to\hspace{0.2cm}(n - 1)$

### Reversing elements of list
`Collections` class defines the following reverse method to reverse elements of a list:
#### 1.
```java
public static void reverse(List list);
```

#### reverse() vs reverseOrder()
We can use `reverse()` method to reverse order of elements of given list whereas we can use `reverseOrder()` method to get reverse `Comparator` object.



{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}
{% include mathjax.html %}