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

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}