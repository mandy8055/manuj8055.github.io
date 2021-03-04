---
title: Map interface in Java
tags: [Core java, Collections]
description: Introduction to Map and various classes implementing Map interface in java Collections framework.
comments: true
mathjax: false
style:
color:
---

## Introduction to Map
- `Map` is **not** the child interface of `Collection`.
- If we want to represent a group of objects as *key-value pairs*, then we should go for `Map`.
- Both *keys* and *objects* are **objects** only.
- Duplicate keys are not allowed but values can be duplicated.
- Each key-value pair is called **entry**, hence `Map` is considered as collection of **`entry`** objects.

### Map interface methods
##### 1.
```java
 Object put(Object key, Object value);
```
- To add one key-value pair to the map.
- If the key already present then, old value will be replaced with the new value and <mark style="background-color: azure">old value will be returned.</mark>

##### 2. Utility methods
```java
void putAll(Map m); // Merge small map into large Map.
Object get(Object key); // Get the value corresponding to the key.
Object remove(Object key); // Remove the entry from the Map.
boolean containsKey(Object key); // Checks if the Map contains the provided key or not.
boolean containsValue(Object value); // Checks if the Map contains the provided value or not.
boolean isEmpty(); // checks if the Map is empty or not.
int size(); // size of Map.
void clear(); // clears and removes all of the entries from a specified Map.
```

##### 3. Collection views of Map
```java
Set keySet(); // To get only the keys.
Collection values(); // To get all the values.
Set entrySet(); // To get each entry of the specified Map.
```
### Entry interface
- A `Map` is a group of key-value pairs and each key-value pair is called an **entry**. Hence, `Map` is considered as a collection of `entry` objects.
- If `Map` object doesn't exist, then surely there is no chance of existence of `entry` object. Hence, `Entry` interface is defined inside `Map` interface.

```java
interface Map{
    // Some other method signatures.
    interface Entry{
        // Entry specific methods and can be applied specifically for Entry objects
        Object getKey();
        Object getValue();
        Object setValue(Object newObject);
    }
}
```
### Implementation classes of Map interface

#### 1. HashMap
- The underlying data-structure is **hash table**.
- Insertion order is not preserved and it is based on **`hashCode`** of keys.
- Duplicate keys are not allowed but values can be duplicated.
- Heterogeneous objects are allowed for both key and value.
- `null` is allowed for key(only once) and `null` is allowed for values(any number of times).
- `HashMap` implements `Serializable` and `Cloneable` interfaces but not `RandomAccess` interface.
- `HashMap` is the best choice if our frequent operation is search operation.

**NOTE: The constructors provided for `HashMap` is [similar](https://mandy8055.github.io/blog/set-interface-in-java#constructors) to that of `HashSet`. There is only a difference in last constructor i.e. <code>HashMap hashMap = new HasMap(Map m);</code>**

##### Implementation demo
```java
import java.util.*;
public class HashMapDemo {
    public static void main(String[] args){
        HashMap hashMap = new HashMap();// To suppress warning use generics.
        hashMap.put("Mandy", 340); 
        hashMap.put("John", 120);
        hashMap.put("Mike", 500);
        hashMap.put("Ikira", 1000);
        System.out.println(hashMap); // Output order undefined. Based on hashCode value of key.
        System.out.println(hashMap.put("Mandy", 700)); // 340
        Set set = hashMap.keySet();
        System.out.println(set);// Output order undefined. Based on hashCode value of key
        Collection collection = hashMap.values();
        System.out.println(collection);// Output order undefined. Based on hashCode value of key
        Set set1 = hashMap.entrySet();
        System.out.println(set1); // Output order undefined. Based on hashCode value of key
        Iterator itr = set1.iterator();
        while(itr.hasNext()){
            Map.Entry entry = (Map.Entry)itr.next();
            System.out.println(entry.getKey() + "-->" + entry.getValue());// Output order undefined. Based on hashCode value of key
            if(entry.getKey().equals("Ikira")){
                entry.setValue(2500);
            }
        }
        System.out.println(hashMap);// Output order undefined. Based on hashcode value of key
    }
}
```

### Differences between HashMap and HashTable

| HashMap                                                                                                          | HashTable                                                                                                   |
| ---------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| 1. Every method present here is **not synchronized.**                                                            | Every method present here is **synchronized.**                                                              |
| 2. At a time, multiple threads are allowed to operate on `HashMap` object. Therefore, it is **not thread safe**. | At a time, only one threads is allowed to operate on `HashTable` object. Therefore, it is  **thread safe**. |
| 3. Relatively performance is high because threads are not require to wait to operate on `HashMap` object.        | Relatively performance is high because threads require to wait to operate on `HashTable` object.            |
| 4. `null` is allowed for both key and value.                                                                     | `null` is not allowed for keys and values otherwise we will get `NullPointerException`.                     |
| 5. Introduced in *1.2 version* and it is not legacy class.                                                       | Introduced in *1.0 version* and it is a legacy class.                                                       |
 
**NOTE: To get the synchronized version of `HashMap` object, we use `Collections.synchronizedMap(hashMap)` method. The return type of the method is a <code>synchronized Map object</code>.**

#### 2. LinkedHashMap

- It is the child class of `HashMap`.
- It is exactly similar to `HashMap`(including methods and constructors) except the following differences:

| HashMap                                                                       | LinkedHashMap                                                                     |
| ----------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| 1. The underlying data structure is **hash table.**                           | The underlying data structure is a combination of **hash table and linked list.** |
| 2. Insertion order is not preserved and it is based on **hash code** of keys. | Insertion order is preserved.                                                     |
| 3. Introduced in *1.2 version.*                                               | Introduced in *1.4 version.*                                                      |

**NOTE:** In the above program if we replace `HashMap` with `LinkedHashMap` then, output is `{Mandy=340, John=120, Mike=500, Ikira=1000}` i.e. **insertion order is preserved.**

**Note:** `LinkedHashSet` and `LinkedHashMap` are commonly used for developing **cache-based applications.**

#### 3. IdentityHashMap
- It is similar to `HashMap`(including methods and constructors) except the following difference:
  - In the case of normal `HashMap`, **jvm uses `equals()` method** to identify duplicate keys, which is meant for **content comparison**
  - In the case of `IdentityHashMap`, **jvm uses `==` operator** to identify duplicate keys, which is meant for **reference comparision(address comparison).**

##### Implementation demo:

```java
import java.util.HashMap;
import java.util.IdentityHashMap;
public class IdentityHashMapDemo {
    public static void main(String[] args){
        HashMap hashMap = new HashMap();
        IdentityHashMap identityHashMap = new IdentityHashMap();
        Integer i1 = new Integer(10);
        Integer i2 = new Integer(10);
        hashMap.put(i1, "Cydney");
        hashMap.put(i2, "Rhonda");
        System.out.println(hashMap); // {10=Rhonda}
        identityHashMap.put(i1, "Cydney");
        identityHashMap.put(i2, "Rhonda");
        System.out.println(identityHashMap); // {10=Rhonda, 10=Cydney}
    }
}
```

### 4. WeakHashMap
- It is similar to `HashMap` except the following difference:
  - In the case of  `HashMap` even though, object doesn't have any reference; it is not eligible for **garbage collection** if it is associated with `HashMap` i.e. `HashMap` dominates **garbage collector**.
  - In the case of `WeakHashMap`, if object doesn't contain any references; it is eligible for **garbage collection** even though that object is associated with `WeakHashMap` i.e. **garbage collector** dominates `WeakHashMap`.

##### Implementation demo
```java
import java.util.HashMap;
import java.util.WeakHashMap;
public class WeakHashMapDemo {
    public static void main(String[] args) throws Exception {
        HashMap hashMap = new HashMap();
        WeakHashMap weakHashMap = new WeakHashMap();
        TempClass tempClass = new TempClass();
        TempClass tempClass1 = new TempClass();
        hashMap.put(tempClass, "Axle");
        System.out.println(hashMap); // {temp=Axle}
        tempClass = null;
        System.gc();
        Thread.sleep(5000);
        System.out.println(hashMap); // {temp=Axle}
        weakHashMap.put(tempClass1, "Bose");
        System.out.println(weakHashMap);// {temp=Bose}
        tempClass1 = null; 
        System.gc(); // // finalize() method is called
        Thread.sleep(5000);
        System.out.println(weakHashMap); // {}
    }
}
class TempClass{
    public String toString(){
        return "tempClass";
    }
    public void finalize(){
        System.out.println("finalize() method is called");
    }
}
```

### 5. SortedMap
- It is the child interface of `Map`.
- If we want to represent a group of objects as a group of key-value pairs according to some sorting order of **keys**, then we should go for `SortedMap`.
- **Sorting** is based on the key but not value.
- `SortedMap` defines the following specific methods:
```java
// Returns first key of the SortedMap.
public Object firstKey();
// Returns last key of the SortedMap.
public Object lastKey();
// Returns SortedMap whose keys are less than key
public SortedMap headMap(Object key);
// Returns SortedMap whose keys are more than or equal to key.
public SortedMap tailMap(Object key);
// Returns SortedMap whose keys are greater than or equal to key1 and less than key2.
public SortedMap subMap(Object key1, Object key2);
// Returns Comparator object that describes underlying sorting technique. If we are using default natural sorting order then we will get null.
public Comparator comparator();
```

### 6. TreeMap
- The underlying data structure is **Red-Black tree.**
- Insertion order is not preserved and it is based on some sorting order of keys.
- Duplicate keys are not allowed but values can be duplicated.
- If we're depending on default natural sorting order then, keys should be **homogeneous and comparable** otherwise `ClassCastException` occurs.
- If we're defining our own sorting logic by using `Comparator` then, keys need not be homogeneous and comparable.
- Whether we're depending on default natural sorting order or customized sorting order, there are no restrictions for values.

##### null acceptance
- For **non-empty** `TreeMap` if we are trying to insert `null`, then we will get **`NullPointerException`.**
- For **empty** `TreeMap` as the first element `null` is allowed but after inserting that `null` if we are trying to insert any other element, then we will get the runtime exception i.e. `NullPointerException`.

**NOTE 1: Till java _1.6 version_, `null` is allowed as the first element to the empty `TreeMap` but from _1.7 version_ onwards `null` is not allowed even as the first element i.e. '`null` is not applicable for `TreeMap` from _1.7 version_ onwards'.**

**NOTE 2: There is no restriction on presence of `null` in values.**

#### Constructors

##### 1.
```java
TreeMap t = new TreeMap();
```
- The above definition creates an empty `TreeMap` object where the elements will be inserted according to default natural sorting order.

##### 2.
```java
TreeMap t = new TreeMap(Comparator c);
```
- The above definition creates an empty `TreeMap` object where the elements will be inserted according to customized sorting order specified by comparator object `c`.

##### 3.
```java
TreeMap t = new TreeMap(Map map);
```

#### 4.
```java
TreeMap t = new TreeMap(SortedMap sortedMap);
```

In the next post I'll try to share my learnings on `HashTable` and `NavigableHashMap`.


{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}