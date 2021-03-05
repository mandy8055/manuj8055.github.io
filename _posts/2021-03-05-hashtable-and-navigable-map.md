---
title: Hashtable and NavigableSet in java
tags: [Core java, Collections]
description: A brief note on Hashtable and NavigableHashSet in java collections framework.  
comments: true
mathjax: false
style:
color:
---

### Hashtable
- The underlying data structure for `Hashtable` is **hash table.**
- Insertion order is not preserved and it is based on **`hashCode` of keys.**
- Duplicate keys are not allowed and values can be duplicated.
- Heterogeneous objects are allowed for both keys and values.
- `null` is not allowed for both kay and value otherwise we'll get `NullPointerException`.
- It implements `Serializable` and `Cloneable` interfaces but not `RandomAccess` interface.
- Every method present in `Hashtable` is **synchronized** and hence `Hashtable` object is **thread-safe.**
- `Hashtable` is the best choice if our frequent operation is search operation.

#### Constructors
##### 1.
```java
Hashtable h = new Hashtable();
```
- The above definition creates an empty `Hashtable` object with <mark style="background-color: azure">default initial capacity 11</mark> and default [fill ratio(or load factor)](https://www.geeksforgeeks.org/load-factor-and-rehashing/) of **0.75**.

##### 2.
```java
Hashtable h = new Hashtable(int initialCapacity);
```
- The above definition creates an empty `Hashtable` object with specified `initialCapacity` and default fill-ratio.

##### 3.
```java
Hashtable h = new Hashtable(int initialCapacity, float fillRatio);
```
##### 4.
```java
Hashtable h = new Hashtable(Map map);
```
- The above definition creates an equivalent `Hashtable` for the given `Map` c. This constructor is meant for inter-conversion between `Map` objects.

#### Working of HashCodes behind the scene
```java
import java.util.Hashtable;
public class HashtableDemo {
    public static void main(String[] args){
        Hashtable hashtable = new Hashtable();
        hashtable.put(new TempClass1(5), "A");
        hashtable.put(new TempClass1(2), "B");
        hashtable.put(new TempClass1(6), "C");
        hashtable.put(new TempClass1(15), "D");
        hashtable.put(new TempClass1(23), "E");
        hashtable.put(new TempClass1(16), "F");
        System.out.println(hashtable); // {6=C, 16=F, 5=A, 15=D, 2=B, 23=E}
    }
}
class TempClass1{
    int tempHashCode;
    public TempClass1(int tempHashCode){
        this.tempHashCode = tempHashCode;
    }
    public int hashCode(){
        return tempHashCode;
    }
    public String toString(){
        return tempHashCode + "";
    }
}
``` 

{% include elements/figure.html image="https://mandy8055.github.io/assets/2021-03-05-hashtable-opr.png" caption="HashCode working under the hood" %}

**NOTE: The number of buckets depends on the capacity of the `HashTable` class.**

### Properties

If any information changes frequently in the application like username, password, mail-id, etc., they are not recommended to be hard-coded in java file because if in case of any minor change also, to reflect that change; recompilation, rebuild and redeployment of the application is required. Even sometimes, server restart is also needed which creates a huge business impact to the client.

We can overcome this problem by using **properties file.** Such type of variable things can be configured in the properties file. From that properties file, we can read the variables in the java program. The main advantage of this approach is **if there is a change in properties file, redeployment is enough to reflect the change in the application without causing much business damage to the client.**

We can use java `Properties` object to hold properties which are coming from properties file. In normal `Map`(like `HashMap`, `Hashtable`, etc.) object, key and value can be any type but in the case of `Properties`, key and value should be `String` type only.

##### Constructor
```java
Properties properties = new Properties();
```
##### Methods in Properties class
```java
String getProperty(String propertyName); // Gets the property value corresponding to the propertyName.
String setProperty(String propertyName, String propertyValue);// If key is already present, replace the old value with new one and return the old propertyValue.
Enumeration propertyNames(); // Gets the property names.
void load(InputStream is); // to load all the properties from properties file into java properties object.
void store(OutputStream os, String comment); // to store properties from java properties object into properties file.
```

#### Implementation demo
```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.util.Properties;
public class PropertiesDemo {
    public static void main(String[] args) throws Exception{
        Properties properties = new Properties();
        FileInputStream fis = new FileInputStream("your_path/info.properties");
        properties.load(fis);
        System.out.println(properties);
        String string = properties.getProperty("SomeOtherInformation");
        System.out.println(string);
        properties.setProperty("DBPass", "SomePass");
        FileOutputStream fileOutputStream = new FileOutputStream("your_path/info.properties");
        properties.store(fileOutputStream, "Updated info.properties and performed housekeeping");
    }
}
```
**NOTE: For maintaining db connections and various other enterprise applications, we use `Properties` class.**

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}