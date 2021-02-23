---
title: Examples on finding time complexity of Iterative programs-1
tags: [Algorithms, Time complexity analysis]
style:
color:
mathjax: true
description: A well-known set of examples for finding the time complexity of Iterative programs.
comments: true
---

## Brief primer on Time Complexity

Whenever we try to write the solution for any problem statement, it is very important to analyse how much time the code takes to execute on the computer system and how much amount of memory of the system it is consuming. One thing to note here is that we are not always interested in finding the exact amount of time or memory **but the approximation**. This is the reason we normally talk about asymptotic analysis and the math behind them whenever we discuss performance of some code or algorithm.

#### Asymptotic Notations

- **Big $\mathcal{O}$ notation**: It represents the <mark style="background-color: azure">upper bound or the worst case of any function</mark> on input n. For example, if f(n) = $\mathcal{O}$(g(n)), then g(n) will always be asymptotically greater than or equal to f(n).
- **Big $\Omega$ notation**: It represents the <mark style="background-color: azure">lower bound or the best case of any function</mark> on input n. For example, if f(n) = $\Omega$(g(n)), then g(n) will always be asymptotically smaller than or equal to f(n).
- **Big $\Theta$ notation**: When any two functions are asymptotically equal for the given positive input n, we can represent it using $\Theta$ notation. In simple words <mark style="background-color: yellow">if Big O and Big $\Omega$ for any two comparable functions is possible then only $\Theta$ is possible.
- **Small $\mathcal{o}$ notation**: It represents the <mark style="background-color: azure">upper bound or the worst case of any function</mark> on input n. For example, if f(n) = $\mathcal{o}$(g(n)), then g(n) will always be **strictly** asymptotically greater than f(n).
- **Small $\omega$ notation**: It represents the <mark style="background-color: azure">lower bound or the best case of any function</mark> on input n. For example, if f(n) = $\omega$(g(n)), then g(n) will always be **strictly** asymptotically smaller than f(n).

**Note 1:** If small $\mathcal{o}$ or small $\omega$ is possible then $\Theta$ is not possible.

**Note 2:** We generally discuss about worst case complexity, therefore **Big $\mathcal{O}$** is mostly used to report time and space complexities.

## Major time complexity classes for Iterative programs

### 1. $\mathcal{O}(1)$ time complexity

- Method/function without any loop or **recursive call**(discussed in later post):

```java
public static void main(String[] args){
    System.out.println("Mandy8055");
}
```

- Method/function with loop but constant upper bound:

```java
public static void main(String[] args){
    for(int i = 1; i < 1000000000; i++)
        System.out.println("Mandy8055");
}
```

### 2. $\mathcal{O}(n)$ time complexity

- One for/while/do-while loop with variable/program independent upper bound:

```java
public static void main(String[] args){
    int x, y, z;
    x = y + z; // 1
    for(int i = 1; i < n; i++)
        System.out.println("Mandy8055"); // n
    // Resultant n + 1 or O(n)
}
```

- Multiple non-nested for/while/do-while loop with variable/program independent upper bound:

```java
public static void main(String[] args){
    int j = 0;
    for(int i = 1; i < n; i++)
        System.out.println("Mandy8055"); // n times
    while(j < n){
       System.out.println("Again");  // n times
       j++; 
    }
    // Resultant 2n times or O(n)
}
```

- When increment is addition of some constant:

```java
public static void main(String[] args){
    for(int i = 1; i < n; i = i + 23)
        System.out.println("Mandy8055");  // n / 23 times or O(n)
}
```

- When decrement is subtraction of some constant:

```java
public static void main(String[] args){
    for(int i = n; i > 0; i = i - 23)
        System.out.println("Mandy8055");  // n / 23 times or O(n)
}
```

- **Some more examples**

**1. Example 1**

```java
public static void main(String[] args){
    int i = 1;
    while(i < n){
        i += 4;
        i += 6;        // Combined O(n / 11) or O(n)
        i++;
        System.out.println("Mandy8055");
        // Same is applicable for subtraction
    }
}
```

**2. Example 2** 

```java
public static void main(String[] args){
    int i = 1;
    while(i < n){
        i += 2;
        i -= 4;        // Combined O(n / 8) or O(n)
        i += 10;
        System.out.println("Mandy8055");
    }
}
```

**3. Example 3(_CAVEAT_)**

```java
public static void main(String[] args){
    int i = 1;
    while(i < n){
        i += 2;
        i -= 6;        // Combined n - 3
        i += 1;
        System.out.println("Mandy8055");
    }
    // Infinite loop
}
```

### 3. $\mathcal{O}(\log n)$ time complexity

- When loop variable is incremented by multiple of some constant. For example in the below snippet the <mark style="background-color: yellow">loop executes $\log_{23}n$ or $\mathcal{O}(\log n)$ times.</mark>

```java
public static void main(String[] args){
    for(int i = 1; i < n; i = i * 23)
        System.out.println("Mandy8055");
}
```
- When loop variable is decremented by division of some constant. For example in the below snippet the <mark style="background-color: yellow">loop executes 
$\log_{23}n$ or $\mathcal{O}(\log n)$ times.</mark>

```java
public static void main(String[] args){
    int i = n;
    while(i > 0){
        System.out.println("Mandy8055");
        i /= 23;
    }
}
```

**Some more examples:**

**Example 1: Loop executes <mark style="background-color: azure">$\log_{60}n$ times</mark>:**

```java
public static void main(String[] args){
    int i = 0;
    while(i < n){
        System.out.println("Mandy8055");
        i *= 12;
        i *= 5;
    }
}
```

**Example 2: Loop executes <mark style="background-color: azure">$\log_{6}n$ times</mark>:**

```java
public static void main(String[] args){
    int i = 0;
    while(i < n){
        System.out.println("Mandy8055");
        i *= 12;
        i *= 5;
        i /= 10;
    }
}
```

**Example 3: CAVEAT for <mark style="background-color: azure">infinite loop</mark>:**
```java
public static void main(String[] args){
    int i = n;
    while(i > 0){
        System.out.println("Mandy8055");
        i *= 12;
        i *= 5;
        i /= 10;
    }
}
```

### 4. $\mathcal{O}(\log\log n)$ time complexity

- When the loop variable is incremented in terms of its constant powers. For example, in the below snippet the <mark style="background-color: yellow">loop executes $\log_{29}\log_{12} n$ or time complexity becomes $\log\log n$.</mark>

```java
public static void main(String[] args){
    int i = 12;
    while(i < n){
        System.out.println("Mandy8055");
        i = (int)Math.pow(i, 29);
    }
}
```

- When the loop variable is decremented in terms of its constant fractional powers. For example, in the below snippet the <mark style="background-color: yellow">loop executes $\log_{2}\log_{12} n$ or time complexity becomes $\log\log n$.</mark>

```java
public static void main(String[] args){
    int i = n;
    while(i > 12){
        System.out.println("Mandy8055");
        i = (int)Math.sqrt(i);
    }
}
```

**One more example:**

**Example 1: Loop executes <mark style="background-color: azure">$(\log_{2}\log_{3} n)$ times</mark>:**

```java
public static void main(String[] args){
    int i = 3;
    while(i < n){
        i = (int)Math.pow(i, 4);
        i = (int)Math.sqrt(i);
        // The above two statements combined make the value of counter variable to increment as i^2.
    }
}
```

### 5. $\mathcal{O}(n^c)$ time complexity

The nested **independent** loops generally incur the time complexity as $\mathcal{O}(n^c)$ where **c is some constant**. By independent I mean that the counter or loop variables for the outer as well as the inner loop share no relationship with each other. Let us see some examples to get the clear idea.

**Example 1: Notice there is no relationship between outer and inner loop counter variables <code>i</code> and <code>j</code>. The <mark style="background-color: azure">complexity of the below code snippet becomes $\mathcal{O}(n^2)$</mark>:**

```java
public static void main(String[] args){
    for(int i = 0; i < n; i++)
        for(int j = 0; j < n; j++){
            System.out.println("Mandy8055");
        }
}
```

**Example 2: A slight variation. The <mark style="background-color: azure">complexity becomes $\mathcal{O}(n^3)$</mark>:**

```java
public static void main(String[] args){
    for(int i = 0; i < n; i++)
        for(int j = 0; j < n*n; j++){
            System.out.println("Mandy8055");
        }
}
```

**Example 3: To calculate the time complexity of below code snippet convert the loop condition in terms of n. <mark style="background-color: yellow">The time complexity becomes $\mathcal{O}(n^{3/2})$.</mark>**

```java
public static void main(String[] args){
    for(int i = 0; i < n; i++)
        for(int j = 0; j * j < n; j++){ // j runs root(n) times or n^(1/2) times
            System.out.println("Mandy8055");
        }
}
```

In the next post; I share some of my insights on **dependent nested loops** which are much more interesting and fun to calculate.

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}
{% include mathjax.html %}