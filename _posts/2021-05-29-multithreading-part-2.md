---
title: Multi-threading part 2
tags: [Java, Multi-threading]
style: 
color: 
description: Ways of creating a thread in java.
comments: true
---

In the previous post on multi-threading series, we looked at how multi-tasking is important and how it helps us to increase the overall throughput of the system. In this post, we will focus on how can we create a thread using Java API and which is the recommended way of implementing a thread as per java conventions and why.

## Defining a Thread in Java
We can define a thread in the following two ways:
1. By extending **Thread** class.
2. By implementing **Runnable** interface.

### Defining a Thread by extending Thread class

```java
class MyThread extends Thread{
    @Override
    public void run(){
        // Below for loop is called job of a thread and it is executed by child thread
        for(int i = 0; i < 3; i++){
            System.out.println("child Thread");
        }
    }
}

class ThreadDemo{
    public static void main(String[] args){
        MyThread t1 = new MyThread(); // Thread instantiation done by main thread. This is the reason t1 becomes child thread.
        t1.start(); // Starting of a thread.
        for(int i = 0; i < 3; i++)
            System.out.println("main thread"); // executed by main thread.
    }
}
```

#### Case study for the above way

##### Case 1: Thread Scheduler
- Thread scheduler is the part of JVM.
- It is responsible to schedule threads i.e. if multiple threads are waiting to get the chance of execution, then in which order threads will be executed is decided by **Thread scheduler.**
- We cannot expect exact algorithm followed by Thread scheduler. It is varied from JVM to JVM. Hence, we cannot expect thread's execution order and exact output. Although, several possible outputs can be decided.

##### Case 2: Calling t1.start() vs calling t1.run()
In the case of `t.start()`, a new thread will be created, which is responsible for the execution of `run()` method. But, in the case of `t.run()`, a new thread won't be created and `run()` method will be executed just like a normal method call by **main thread**. Hence, in the above program if we replace `t.start()` with `t.run()`, then the output is:
```java
Child Thread
Child Thread
Child Thread
Main Thread
Main Thread
Main Thread
```
The above output is produced by only **main thread**. 

##### Case 3: Importance of Thread class start() method
`Thread` class `start()` method is responsible to register thread with thread scheduler and other mandatory activities. Hence, without executing thread class start method, there is no chance of starting a new thread in java, Due to this, `Thread` class `start()` method is considered as **heart of multi-threading**.
```java
start(){
    // Step 1: Register this method with Thread scheduler.
        // Body
    // Step 2: perform all other mandatory activities.
        // Body
    // Step 3: Invoke run() method.
        // Body
}
```

##### Case 4: Overloading of run() method
Overloading of `run()` method is possible but `Thread` class `start()` method will invoke **no-argument** run method. The other overloaded method can be called explicitly like a normal method call.
```java
class MyThread extends Thread
{
    public void run(){
        System.out.println("no-arg run method");
    }
    // overloaded run method
    public void run(int i){
        System.out.println("int-arg run method");
    }
}

class ThreadDemo{
    public static void main(String[] args){
        MyThread t1 = new MyThread(); // Thread instantiation done by main thread. This is the reason t1 becomes child thread.
        t1.start(); // Starting of a thread.

        // Output: no-arg run mathod
    }
}
```

##### Case 5: Overriding run() method
If we are not overriding `run()` method, then `Thread` class `run()` method will be executed which has empty implementation. Hence, we won't get any output.
**NOTE:** It is highly recommended to override `run()` method.

##### Case 6: Overriding of start() method
If we override `start()` method then, it will be executed just like a normal method call and new thread won't be created.
```java
class MyThread extends Thread{
    public void start(){
        System.out.println("start method");
    }
    public void run(){
        System.out.println("run method");
    }
}

class Test{
    public static void main(String[] args){
        MyThread t = new MyThread();
        t.start();
        System.out.println("main method");
        // Output: 
        // start method
        // main method
    }
}
```
**NOTE:** It is not recommended to override `start()` method.

##### Case 7: 
```java
class MyThread extends Thread{
    public void start(){
        super.start();
        System.out.println("start method");
    }
    public void run(){
        System.out.println("run method");
    }
}

class Test{
    public static void main(String[] args){
        MyThread t = new MyThread();
        t.start();
        System.out.println("main method");
        //Possible Output:
        /*
            Output 1: 
            run method
            start method
            main method
            Output 2: 
            start method
            main method
            run method
            Output 3:
            start method
            run method
            main method 

            Run method is executed by child thread
            start method and main method is executed by main thread.
        */
        
    }
}
```
##### Case 8: Restarting the same
After starting a thread if we are trying to restart the same thread then, we'll get runtime exception saying `IllegalThreadStartException`.
```java
Thread t = new Thread();
t.start();
// Some statements
t.start(); // Runtime Exception: IllegalThreadStartException
```

### Defining a Thread by implementing Runnable interface
- We can define a thread by implementing `Runnable` interface. 
- `Runnable` interface is present in `java.lang` package.
- `Runnable` interface contains only one method which is `run()` method.

```java
// Defining a thread
class MyRunnable implements Runnable{
    // Job of a thread
    public void run(){
        for(int i = 0; i < 3; i++){
            System.out.println("child thread");
        }
    }
}

class ThreadDemo{
    public static void main(String[] args){
        MyRunnable r = new MyRunnable();
        Thread t = new Thread(r); // r is called target runnable
        t.start();
        for(int i = 0; i < 3; i++)
            System.out.println("main thread"); // executed by main thread.
        // output is unpredictable.
    }
}
```
#### Important Cases
```java
MyRunnable r = new MyRunnable();
Thread t1 = new Thread();
Thread t2 = new thread(r);
```
Considering the above example, below are the important conclusions:

1. **<code>t1.start()</code>**: A new thread will be created and which is responsible for the execution of `Thread` class `run()` method, which has empty implementation.
2. **<code>t1.run()</code>**: No new thread will be created and `Thread` class `run()` method will be executed just like a normal method call. 
3. **<code>t2.start()</code>**: A new thread will be created which is responsible for the execution of `MyRunnable` class `run()` method.
4. **<code>t2.run()</code>**: A new thread won't be created and `MyRunnable` class `run()` method will be executed just like a normal method call.
5. **<code>r.start()</code>**: We will get compile-time error saying `MyRunnable` class doesn't have start capability.
6. **<code>r.run()</code>**: No new thread will be created and `MyRunnable` class `run()` method will be executed like the normal method call.

### Which approach is best to define a thread in java
Among the above two way of defining a thread, `implements Runnable` approach is recommended because in the first approach, child class always extends `Thread` class. There is no chance of extending any other class. Hence, we are **missing inheritance benefit.** But in the second approach, while implementing `Runnable` interface we can extend any other class. Hence, we will not miss any inheritance benefit.

Because of above reason, implementing `Runnable` interface approach is recommended than extending `Thread` class.

In the upcoming blog I'll try to share my learning on `Thread` class constructors, priorities and importance of priorities in multi-threading.

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}