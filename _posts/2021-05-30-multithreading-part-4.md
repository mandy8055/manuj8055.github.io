---
title: Multi-threading part 4
tags: [Java, Multi-threading]
style: 
color: 
description: A descriptive information about yield, join and sleep methods and gist on life-cycle of a thread. 
comments: true
---

In the previous post we saw about priorities of a thread and how are they defined and handled. In this post we will see what are **<code>yield()</code>**, **<code>join()</code>** and **<code>sleep()</code>** methods. We will also look at the lifecycle of a thread in its entirety.

## yield() method

- This method causes  <mark style="background-color: yellow">to pause the current executing thread to give the chance for waiting threads of same priority.</mark> 
- If there is no waiting thread or all waiting threads have low priority, then same thread can continue its execution.
- If multiple threads are waiting with same priority, then we cannot expect which of the waiting thread will get the chance for execution. 
- It solely depends on thread scheduler. The thread which is **yielded**, when it will get the chance again will solely depend on thread scheduler.
- Syntax of `yield()` is **<code>public static native void yield();</code>**. It is not implemented in java since it is **native**.

Let us take the same old example to understand the working of `yield()` method.
```java
class MyThread extends Thread{
    @Override
    public void run(){
        // Below for loop is called job of a thread and it is executed by child thread
        for(int i = 0; i < 3; i++){
            System.out.println("child Thread");
            Thread.yield();
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

In the above program, if we are commenting **<code>Thread.yield()</code>**, then both threads will be executed simultaneously and we cannot expect which thread will complete first.

If we are not commenting **<code>Thread.yield()</code>**, then `child` thread always calls `yield()` and because of that `main` thread will get chance more number of times and the chance of completing `main` thread first is high.

**NOTE:** Some platforms will not provide proper support for `yield()`.

## join() method
If a thread wants to wait until completing some other thread, then we should go for `join()` method. for example, if a thread **<code>t1</code>** wants to wait until completing **<code>t2</code>**, then **<code>t1</code>** has to call **<code>t2.join()</code>**. If **<code>t1</code>** executes **<code>t2.join()</code>**, then immediately **<code>t1</code>** will enter into **waiting state** until **<code>t2</code>** completes. Once **<code>t2</code>** completes, then **<code>t1</code>** can continue its execution. Various prototypes of `join()` is:

> **<code>public final void join()throws InterruptedException</code>**

> **<code>public final void join(long msec)throws InterruptedException</code>**

> **<code>public final void join(long msec, int ns)throws InterruptedException</code>**

**NOTE:** Every `join()` method throws `InterruptedException` which is a **checked exception.** Hence, we need to handle this exception compulsorily either by using `try-catch` or by using `throws` keyword, otherwise we will get compile-time error.

## Important case studies

### Case 1: Waiting of main thread until completing child thread
```java
class MyThread extends Thread{
    public void run(){
        for(int i = 0; i < 3; i++){
            System.out.println("A thread");
            try{
                Thread.sleep(2000);
            }catch(InterruptedException e){}
        }
    }
}

class ThreadJoinDemo{
    public static void main(String[] args)throws InterruptedException{
        MyThread t = new MyThread();
        t.start();
        t.join();
        // t.join(10000);
        for(int i = 0; i < 3; i++){
            System.out.println("main thread");
        }
    }
}
```

- If we comment **<code>t.join();</code>**, then both `main` and `child` threads will be executed simultaneously and we cannot expect exact output.
- If we are not commenting **<code>t.join();</code>**, then `main` thread calls `join()` method on `child` thread object hence, `main` thread will wait until completing `child` thread. In this case output is; 
```java
A thread
A thread
A thread
main thread
main thread
main thread
```

### Case 2: Waiting of child thread until completing main Thread
```java
class MyThread extends Thread
{
    static Thread mainThread;
    public void run(){
        try{
            mainThread.join(); // child thread waits for main thread to complete
        }catch(InterruptedException e){}
        for(int i = 0; i < 3; i++){
            System.out.println("Child Thread");
        }

    }
}

class ThreadJoinDemo{
    public static void main(String[] args)throws InterruptedException{
        MyThread.mainThread = Thread.currentThread();
        MyThread t = new MyThread();
        t.start();
        for(int i = 0; i < 3; i++){
            System.out.println("Main Thread");
            Thread.sleep(2000);
        }
    }
}
```
- In the above example, `child` thread calls `join()` method on `main` thread object. Hence, `child` thread has to wait until completing `main` thread. In this case output is:
```java
Main Thread
Main Thread
Main Thread
Child Thread
Child Thread
Child Thread
```

### Case 3: When main and child thread call join on each other
```java
class MyThread extends Thread
{
    static Thread mainThread;
    public void run(){
        try{
            mainThread.join(); // child thread waits for main thread to complete
        }catch(InterruptedException e){}
        for(int i = 0; i < 3; i++){
            System.out.println("Child Thread");
        }

    }
}

class ThreadJoinDemo{
    public static void main(String[] args)throws InterruptedException{
        MyThread.mainThread = Thread.currentThread();
        MyThread t = new MyThread();
        t.start();
        t.join();
        for(int i = 0; i < 3; i++){
            System.out.println("Main Thread");
            Thread.sleep(2000);
        }
    }
}
```
- If `main` thread calls `join()` on `child` thread object and `child` thread calls `join()` on `main` thread object, then both threads will wait forever and the program goes in **deadlock state**.

### Case 4: When a thread applies join on itself
```java
class Test{
    public static void main(String[] args)throws InterruptedException{
        Thread.currentThread().join();
    }
}
```
- If a thread calls `join()` on itself, then the program goes in **deadlock state**. In this case, the thread has to wait infinite amount of time.

## sleep() method
If a thread don't want to perform any operation for a particular amount of time, then we should go for `sleep()` method.

> **<code>public static native void sleep(long msec)throws InterruptedException</code>**

> **<code>public static void sleep(long msec, int ns)throws InterruptedException</code>**

**NOTE:** Every `sleep()` method throws `InterruptedException`, which is a **checked exception** and therefore, whenever we are using `sleep()` method we should handle `InterruptedException` either by `try-catch` or by `throws` keyword compulsorily otherwise we will get compile time error.

Let us take a demo example to understand the `sleep()` method.
```java
class SlideRotator{
    public static void main(String[] args)throws InterruptedException{
        for(int i = 1; i<= 3; i++){
            System.out.println("Slide-" + i);
            Thread.sleep(5000);
        }
    }
}
```

## interrupt() method
A thread can interrupt a sleeping thread or a waiting thread by using **<code>interrupt()</code>** method of `Thread` class.

> **<code>public void interrupt()</code>**

Let us look at an example to know how can we apply this method to interrupt a thread.
```java
class MyThread extends Thread{
    public void run(){
        try{
            for(int i = 0; i < 3; i++){
                System.out.println("I am a lazy thread.");
                Thread.sleep(2000);
            }
        }catch(InterruptedException e){
            System.out.println("I got interrupted.");
        }
    }
}
class ThreadInterruptDemo{
    public static void main(String[] args){
        MyThread t = new MyThread();
        t.start();
        t.interrupt(); // main thread executes this line
        System.out.println("End of main");
    }
}
```
- If we comment **<code>t.interrupt();</code>**, then `main` thread will not interrupt `child` thread. In this case `child` thread will execute `for` loop 3 times.
- If we are not commenting **<code>t.interrupt();</code>**, then `main` thread interrupts `child` thread. In this case, output is:
```java
End of main
I am a lazy thread.
I got interrupted.
```

<mark style="background-color: yellow;font-weight: bold">NOTE:</mark> Whenever we are calling `interrupt()` method and if the target thread is not in sleeping or waiting state, then there is no impact of interrupt call immediately. Interrupt call will be waited until target thread enters into sleeping or waiting state. If the target thread enters into sleeping or waiting state, then immediately interrupt call will interrupt the target thread.

If the target thread never entered into sleeping or waiting state in its life-time in its lifetime, then there is no impact of interrupt call. This is the only case where interrupt call will be wasted.

```java
class MyThread extends Thread{
    public void run(){
        for(int i = 1; i <= 5000; i++){
            System.out.println("I am a lazy thread-" + i);
        }
        System.out.println("I am entering into sleeping state.");
        try{
            Thread.sleep(10000);
        }catch(InterruptedException e){
            System.out.println("I got interrupted.");
        }
    }
}
class ThreadInterruptDemo{
    public static void main(String[] args){
        MyThread t = new MyThread();
        t.start();
        t.interrupt(); // main thread executes this line
        System.out.println("End of main");
    }
}
```
- In the above example, interrupt call waited until child thread completes `for` loop **5000 times**.

## Conclusion[tl;dr]

| Property                                 | yield()                                                                                             | join()                                                        | sleep()                                                                           |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| **PURPOSE**                              | If a thread wants to pause its execution to give the chance for remaining threads of same priority. | If a thread wants to wait until completing some other thread. | If a thread do not want to perform any operation for a particular amount of time. |
| **IS IT OVERLOADED?**                    | NO                                                                                                  | YES                                                           | YES                                                                               |
| **IS IT FINAL?**                         | NO                                                                                                  | YES                                                           | NO                                                                                |
| **DOES IT THROWS InterruptedException?** | NO                                                                                                  | YES                                                           | YES                                                                               |
| **IS IT NATIVE METHOD?**                 | YES                                                                                                 | NO                                                            | `sleep(long msec)` is native but `sleep(long msec, int ns)` is not native         |
| **IS IT STATIC?**                        | YES                                                                                                 | NO                                                            | YES                                                                               |


This was all about `yield()`, `join()` and `sleep()` methods of `Thread`class. In further upcoming blogs I'll share my insights on one of most important and fun topic in multi-threading i.e. **synchronization**.

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}