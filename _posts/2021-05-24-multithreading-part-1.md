---
title: Multi-threading part 1
tags: [Java, Multi-threading]
style: 
color: 
description: Introduction to multi-tasking and why we require multi-tasking.
comments: true
---

In upcoming blogs, I'll try to share my learnings on one of the most important concept in operating system i.e. **multi-threading**. It is one of key concept in java also. But, before diving into details of multi-threading let us understand why this concept came, what is multi-tasking systems and why are they useful.

### Multi-tasking
Executing several tasks simultaneously is the concept of **multi-tasking**. There are two types of multi-tasking:
- Process-based multi-tasking
- Thread-based multi-tasking

#### Process-based Multi-tasking
Executing several tasks simultaneously where each task is a separate independent program(process) is called process-based multi-tasking.
e.g. While typing a java program in the editor, we can listen our favorite spotify playlist from same system. At the same time we can download a file. All these tasks will be executed simultaneously and independent of each other. Hence, it is process-based multi-tasking. *Process based multi-tasking is best suitable at OS level.*

#### Thread-based Multi-tasking
Executing several task simultaneously where each task is a separate independent part of the same program is called thread-based multi-tasking and each independent part is called a **thread**. *Thread-based multi-tasking is best suitable at programmatic level.*

Whether it is process-based or thread-based, the main objective of multi-tasking is to **reduce response-time** of the system and to **improve performance**.

Important application areas of multi-threading are:
1. To develop multi-media graphics.
2. To develop animations.
3. To develop games.
4. To develop web-servers and application servers etc.

When compared with old languages, developing multi-threaded applications in java is very easy because java provides inbuilt support for multi-threading with rich API like `Thread`, `ThreadGroup`, `Runnable`, etc. In the next blog, I'll try to share my learning on methods to create a thread in java.

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}