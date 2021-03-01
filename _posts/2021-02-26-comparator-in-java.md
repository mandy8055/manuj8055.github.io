---
title: Comparator in java Collection framework
tags: [Core java, Collections]
description: A brief note on Comparator interface in java Collection framework.
comments: true
mathjax: false
style:
color:
---

## Comparator Interface
- `Comparator` interface is present in `java.util` package and it contains two methods which are `compare()` and `equals()`.
- Whenever we are implementing `Comparator` interface, we should provide the implementation for `compare()` compulsorily and we are not required to provide implementation for `equals()` because it is already available to our class from `Object` class through **inheritance.**

#### Implementation Demo 1

> WAP to insert integer objects into the `TreeSet` where the sorting order is descending.

```java
import java.util.Comparator;
import java.util.TreeSet;
public class ComparatorDemo1 {
    public static void main(String[] args){
        TreeSet treeSet = new TreeSet(new MyComparator()); // To suppress warning use Generics concept
        treeSet.add(10);
        treeSet.add(0);
        treeSet.add(15);
        treeSet.add(5);
        treeSet.add(20);
        treeSet.add(20);
        System.out.println(treeSet); // [20, 15, 10, 5, 0]
    }
}
class MyComparator implements Comparator{
    @Override
    public int compare(Object o1, Object o2) {
        Integer I1 = (Integer) o1;
        Integer I2 = (Integer) o2;
        if(I1 < I2){
            return 1;
        }else if(I1 > I2){
            return -1;
        }else return 0;
    }
}
``` 
**NOTE:** At the line <code>TreeSet treeSet = new TreeSet(new MyComparator());</code>, if we are not passing `MyComparator` object then internally **JVM** will call `compareTo()` which is meant for *default natural sorting order*.

#### Various implementations of compare() method

```java
@Override
public int compare(Object o1, Object o2) {
    Integer I1 = (Integer) o1;
    Integer I2 = (Integer) o2;
    /* Case 1: */ return I1.compareTo(I2); // [0, 5, 10, 15, 20]
    /* Case 2: */ return -(I1.compareTo(I2)); // [20, 15, 10, 5, 0]
    /* Case 3: */ return I2.compareTo(I1); // [20, 15, 10, 5, 0]
    /* Case 4: */ return -(I2.compareTo(I1)); // [0, 5, 10, 15, 20]
    /* Case 5: */ return 1; // Insertion order including duplicates i.e. [10, 0, 15, 5, 20, 20]
    /* Case 6: */ return -1; // reverse of insertion order including duplicates i.e. [20, 20, 5, 15, 0, 10]
    /* Case 7: */ return 0; // first element [10] since all the remaining elements will be duplicates because returned value is 0.   
}
```
#### Implementation Demo 2
> WAP to insert `String` objects into the `TreeSet` where all elements should be inserted according to reverse of alphabetical order.

```java
import java.util.Comparator;
import java.util.TreeSet;
public class ComparatorDemo1 {
    public static void main(String[] args){
        TreeSet treeSet = new TreeSet(new MyComparator1()); // To suppress warning use Generics concept
        treeSet.add("Romesh");
        treeSet.add("Sandy");
        treeSet.add("Rammy");
        treeSet.add("Geeta");
        treeSet.add("Raja");
        System.out.println(treeSet); // [Sandy, Romesh, Rammy, Raja, Geeta]
    }
}
class MyComparator1 implements Comparator{
    @Override
    public int compare(Object o1, Object o2){
        // Both the below ways can be used to convert object to String.
        String s1 = (String)o1; // favorable when converting any type of object to String.
        String s2 = o2.toString(); // favorable when converting from StringBuffer, StringBuilder to String.
        return s2.compareTo(s1);
    }
}
```

#### Implementation Demo 3
> WAP to insert `StringBuffer` objects into the `TreeSet` where the sorting order is alphabetical order.

**Since the `StringBuffer` objects are not <mark style="background-color: azure">comparable</mark>, we cannot use the default natural sorting order (`ClassCastException`), so definitely we require customized sorting order.**

```java
import java.util.Comparator;
import java.util.TreeSet;
public class ComparatorDemo3{
    public static void main(String[] args){
        TreeSet treeSet = new TreeSet(new MyComparator3());
        treeSet.add("Romesh");
        treeSet.add("Sandy");
        treeSet.add("Rammy");
        treeSet.add("Geeta");
        treeSet.add("Raja");
        System.out.println(treeSet); // [Geeta, Raja, Rammy, Romesh, Sandy]
    }
}
class MyComparator3 implements Comparator{
    @Override
    public int compare(Object o1, Object o2) {
        String s1 = (String)o1;
        String s2 = o2.toString();
        return s2.compareTo(s1);
    }
}
```
**Note:** If we are depending on default natural sorting order then, the objects should be **homogenous** and **comparable** compulsorily.

If we are defining our own sorting logic by using `Comparator`, then objects need not be **comparable** and **homogenous** i.e. we can add heterogenous, non-comparable objects also.

#### Implementation Demo 4
> WAP to insert `String` and `StringBuffer` objects into `TreeSet` where sorting order is increasing length order. If two objects having same length, then consider their alphabetical order.

```java
import java.util.Comparator;
import java.util.TreeSet;
public class ComparatorDemo4 {
    public static void main(String[] args){
        TreeSet treeSet = new TreeSet(new MyComparator4());
        treeSet.add("A");
        treeSet.add(new StringBuffer("ABC"));
        treeSet.add(new StringBuffer("AA"));
        treeSet.add("XX");
        treeSet.add("ABCD");
        treeSet.add("A");
        System.out.println(treeSet); // [A, AA, XX, ABC, ABCD]
    }
}
class MyComparator4 implements Comparator{
    @Override
    public int compare(Object o1, Object o2) {
        String s1 = o1.toString();
        String s2 = o2.toString();
        int l1 = s1.length();
        int l2 = s2.length();
        if(l1 < l2)return -1;
        else if(l1 > l2)return 1;
        else
            return s1.compareTo(s2);
    }
}
```

#### Comparable vs Comparator
- For **predefined comparable classes**, default natural sorting order already available. If we don't want default natural sorting order, then we can define our own sorting logic by using `Comparator`.
- For **predefined non-comparable classes(like `StringBuffer`)**, default natural sorting order is not available. We can define our own sorting logic by using `Comparator`.
- For **user-defined classes** like `Employee`, the developer(ourselves) is responsible to define default natural sorting order by implementing `Comparable` interface. The client using the class, if not satisfied with default natural sorting order, then the client can define his own sorting by using `Comparator`.

#### Implementation demo

```java
import java.util.Comparator;
import java.util.TreeSet;
public class Employee implements Comparable{
    int eid;
    String name;
    public Employee(int eid, String name){
        this.eid = eid;
        this.name = name;
    }
    public String toString() {
        return eid + " --> " + name;
    }
    @Override
    public int compareTo(@NotNull Object o) {
        Employee i1 = (Employee) o;
        if(this.eid < i1.eid)return -1;
        else if(this.eid > i1.eid) return 1;
        else return 0;
    }
}
class ComparatorVsComparable{
    public static void main(String[] args){
        Employee e1 = new Employee(100,"Chirag");
        Employee e2 = new Employee(200,"John");
        Employee e3 = new Employee(50,"Titu");
        Employee e4 = new Employee(150,"Swami");
        Employee e5 = new Employee(100,"Chirag");
        TreeSet treeSet = new TreeSet();
        treeSet.add(e1);
        treeSet.add(e2);
        treeSet.add(e3);
        treeSet.add(e4);
        treeSet.add(e5);
        System.out.println(treeSet);// [50 --> Titu, 100 --> Chirag, 150 --> Swami, 200 --> John]
        TreeSet treeSet1 = new TreeSet(new MyComparator5());
        treeSet1.add(e1);
        treeSet1.add(e2);
        treeSet1.add(e3);
        treeSet1.add(e4);
        treeSet1.add(e5);
        System.out.println(treeSet1);// [100 --> Chirag, 200 --> John, 150 --> Swami, 50 --> Titu]
    }
}
class MyComparator5 implements Comparator{
    @Override
    public int compare(Object o1, Object o2) {
        Employee e1 = (Employee) o1;
        Employee e2 = (Employee) o2;
        return e1.name.compareTo(e2.name);
    }
}
```
### Comparison between Comparator and Comparator

| Comparable                                                          | Comparator                                                                                                                                                                                                    |
| ------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Meant for default natural sorting order                          | Meant for customized sorting order.                                                                                                                                                                           |
| 2. Comparable is present in `java.lang` package.                    | Comparator is present in `java.util` package.                                                                                                                                                                 |
| 3. contains only `compareTo()` method.                              | Initially, two methods were present namely `compare()` and `equals()`. As of **java 1.8** some more [methods](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#method.summary) were added. |
| 4. `String` and all the **wrapper** classes implements `Comparable` | `Collator` and `RuleBasedCollator` classes implements `Comparator`.                                                                                                                                           |

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}

