---
title: Understanding Concurrency and parallelism through java
tags: [java, Multi-threading, concurrency]
style:
color:
description: Distinction between concurrency and parallelism through java language construct.
comments: true
---

<a class="text-center" href="https://feedburner.google.com/fb/a/mailverify?uri=Mandy8055&amp;loc=en_US" onclick="window.open(this.href, 'subscribe',
    'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;">Subscribe for New Posts</a>

---

This post talks about the meaning of **concurrency and parallelism**. It also draws a distinctions between these two. There are times when the concepts which we understand theoretically needs some practical implications. In Operating System, we have studied the meaning of these two terms but more often we tend to confuse when it comes to their implementation. So let us deal these two terms one by one.

## Parallelism
> "Parallelism is about doing a lot of things at once." - Rob Pike

It refers to the technique to make programs faster by performing several computations in parallel. This requires hardware with multiple processing units. Let us take a code to understand better.

```java
public static void main(String[] args){
    // Task 1
    new Thread(new Runnable(){
        @Override
        public void run(){
            generateBill(customer1);
        }
    }).start();
    // Task 2
    new Thread(new Runnable(){
        @Override
        public void run(){
            generateBill(customer2);
        }
    }).start();
    // Task 3
    calculateOverheads();
}
```
In the above code, there are two threads along with the **main** thread. Here, we have three tasks. Task 1 and Task 2 are run on the different threads and the Task 3 is run on the main thread. Now as the definition of parallelism say there needs to be multi-processing units; so let us consider a quad core processor(4 cores).

{% include elements/figure.html image="https://mandy8055.github.io/assets/2020-02-16-image-1.png" caption="How Parallelism works" %}

According to the picture above, let us assume that the thread-1 is being scheduled(by scheduler) on core-1, thread-2 on core-2 and main thread on core-3 thereby running the three tasks parallely. Clearly, these three tasks are independent of each other so they run parallely and hence will increase the degree of multi-programming thereby increasing the efficiency of the computer system.

Parallelism can also be achieved in java using `ThreadPool` class provided by the language construct. We could rewrite the above program as:


#### Conclusion
In java to enable Parallelism; we can use

- Raw threads (as in example 1)
- ThreadPool Class of java
    - ExecutorService
    - ForkJoinPool
    - CustomThreadPools(eg. as used by Web Servers)
- **NOTE: In all the cases we have to ensure that the CPU has more than 1 core.**

## Concurrency
> "Concurrency is about dealing with lot of things at once." - Rob Pike

Concurrency refers to the process when two or more tasks start, run and complete in overlapping time periods. It is an approach of decreasing the response time of the system by using single processing unit. We can think it as an illusion of parallelism, however in actual the chunks of a task aren't parallely processed, but inside the application, there are more than one task that is being processed at a time.

Now, let us look at the code for concurrency in java to get the better insights.

```java
public static void main(String[] args) throws InterruptedException{ 
/* This exception is necessary to be thrown or at least try-catch block is necessarily required
 because in case of any interrupt(that may occur in between the execution of the threads) deadlock may occur even after applying the lock. 
 */
    new Thread(() -> {
        if(ticketsAvailable > 0){
            bookTicket();
            ticketsAvailable--;
        }
    }).start();

    new Thread(() -> {
        if(ticketsAvailable > 0){
            bookTicket();
            ticketsAvailable--;
        }
    }).start();

    Thread.sleep(5000);

}
```

In the above task there are two tasks and both the tasks are accessing the shared resource namely **ticketsAvailable** variable. For the time being let us assume that we have a single core CPU. So, the CPU scheduler will schedule the tasks one at a time in [Round Robin](https://en.wikipedia.org/wiki/Round-robin_scheduling) fashion (i.e. providing each task a fixed time quantum). Concurrent exection of two or more tasks(may it be threads, processes, transactions in database, etc.) are also referred to as interleaving.

Now, since the **execution flow is not guranteed** we cannot conclude that the program instructions (A program is set of instructions which can be [atomic](https://whatis.techtarget.com/definition/atomic) or non-atomic) will run as specified in the code.

Clearly, if the code executes in the above fashion, if the available ticket is 1 then also both the tasks will book the ticket which in turn leads to inconsistency and wrong result. **Thread scehduling is not in the developer's control.** This inconsistency can also occur in multi-core CPU machines. If the two tasks are running parallely on two different cores, we are not sure which instruction is executed by which core at what time in respect of one another.

## The FIX

One of the possible fix for the above problem is the use of **locks**.

```java
public static void main(String[] args){ 
    try{
        new Thread(() -> {
            lock.lock();
            if(ticketsAvailable > 0){
                bookTicket();
                ticketsAvailable--;
            }
        }).start();
    }catch(InterruptedException ex){}
    finally{
        lock.unlock();
    }
    try{
        new Thread(() -> {
            if(ticketsAvailable > 0){
                bookTicket();
                ticketsAvailable--;
            }
        }).start();

        Thread.sleep(5000);
    }catch(InterruptedException ex){}
    finally{
        lock.unlock();
    }

}
```

If you notice the code above, the task which acquires the lock that task will only the execute its instructions(i.e. access the shared resource and updates it) untill and unless it releases its lock thus making the instructions inside the taks atomic. So, the threads are co-ordinating between each other using a single lock to accomplish a certain task.

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}