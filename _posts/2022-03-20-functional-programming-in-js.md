---
title: Functional programming in javascript
tags: [Javascript, Frontend]
style:
color:
description: Understanding functional programming paradigm in javascript
comments: true
---

<a class="text-center" href="https://feedburner.google.com/fb/a/mailverify?uri=Mandy8055&amp;loc=en_US" onclick="window.open(this.href, 'subscribe',
    'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;">Subscribe for New Posts</a>
    
---

Functional programming is one of the many [**programming paradigm**](https://www.geeksforgeeks.org/introduction-of-programming-paradigms/) that is followed in software industry. You probably have heard of **Object oriented programming** which is another programming paradigm that is quite popular. Paradigms generally refer to rules or techniques as to how you should structure your code in order to solve any problem statement. In a nutshell, functional programming is about composing the problem into a bunch of small and reusable functions that takes some inputs and return a result. With the help of these simple functions we can solve complex problems.

The next question that might arise in your mind is *Well! what is the need or benefits of using this paradigm?* The answer is these functions are **concise, easier to debug, easier to test and more scalable.** Apart from this we can also run multiple functions in parallel and take the advantage of different cores of CPU to boost our performance. These are the reasons because of which functional programming has gained quite a bit of ground in recent years. Now there are various languages that are mainly designed for functional programming like **clojure, haskell, etc.**; javascript is a ***multi-paradigm programming language***.

## Functions as first-class variables
In javascript, functions are first-class variables which means: 
- Functions can be assigned to a variable.
- You can pass functions as an argument to other function.
- You can return functions from other function.

```javascript
function greet(func){
    console.log(func());
}
function sayHeyThere(){
    // anonymous function
    return function(){
        return "Hey There";
    }
}
```
## Higher Order functions
In the above example; the functions **<code>greet()</code>** and **<code>sayHeyThere()</code>** have special terminology i.e. *higher order functions.* These are those functions that <mark style = "background-color: yellow">either take other functions as arguments or return functions or both.</mark>They are called so because instead of working on strings, numbers or booleans; it goes higher and operate on functions. You probably have worked on **<code>map()</code>**, **<code>filter()</code>**, **<code>setTimeout()</code>** and **<code>reduce()</code>**. These are higher order functions since they operate on functions.

## Function composition
Let us understand the meaning of function composition with a scenario. Consider we're given a string with padded spaces. We have to trim the input, convert the input to lowercase and wrap a **<code><div></code>** around it. One of the may ways would be:

```javascript
let input = "   Hello World!  ";
let output = `<div>${input.trim().toLowerCase()}</div>`;
```
Hmmm...:thinking: not bad! but the above code is non-functional. Let me try to code above scenario using functional style.

```javascript
import { pipe } from 'loadash/fp';
// trim
const trim = str => str.trim();
// lowercase
const lowerCase = str => str.toLowerCase();
// wrapInDiv
const wrapInDiv = str => `<div>${str}</div>`;

const result = wrapInDiv(lowerCase(trim(input))); // function composition
// To improve readability and remove nested parenthesis we can use loadash
const transform = pipe(trim, lowerCase, wrapInDiv);
transform(input); // <div>hello world!</div>
```
The above code serves as the example of function composition where **<code>wrapInDiv()</code>** function composes **<code>trim()</code>**.

## Currying
In simpler terms, currying is a technique of converting a function with multiple parameters to a function with just one parameter without altering the purpose the former function served. Hmmm...definitions are boring:sleepy:, show me the code example. Let's take a classic example then to study currying in isolation:
```javascript
function add(a, b){
    return a + b;
}
// Currying
function add1(a){
    return function(b){
        return a + b;
    }
}

add1(4)(7); // 11
```
As we can see in the above example, in **<code>add()</code>** function there are two arguments separated by comma but in **<code>add1()</code>** we have converted the **<code>add()</code>** function with same purpose but with single parameter using currying. The next question that arises is *Okay! we converted the function with n arguments to just 1 argument. But what is the advantage.* Let us consider our previous scenario i.e. trim the input, convert the input to lowercase and wrap a **<code><div></code>** around it. Now what if suddenly there comes the requirement that instead of **<code><div></code>**, we need to generalize it to other tags as well i.e. `<span>`. Let me tell you my approach to do it.

```javascript
const wrap = (tag, str)  => `<${tag}>${str}</${tag}>`;
```
But, in the above code there is a problem if we try to integrate it with **<code>loadash/fp pipe()</code>** function. It expects to return a function but returns a string. So, what shall we do:thinking:? You guessed it right, we can use use currying to solve our problem.

```javascript
const wrap = tag => str => `<${tag}>${str}</${tag}>`;
```

```javascript
import { pipe } from 'loadash/fp';
// trim
const trim = str => str.trim();
// lowercase
const lowerCase = str => str.toLowerCase();
// wrap
const wrap = tag => str  => `<${tag}>${str}</${tag}>`;

// To improve readability and remove nested parenthesis we can use loadash
const transform = pipe(trim, lowerCase, wrap('span'));
transform(input); // <span>hello world!</span>
```

## Pure Functions
A pure function is a function to which if we give same argument should always return same result. What do you think about the below example?Is it a pure function?
```javascript
function someFunction(num){
    return num * Math.random();
}
```
Yes, you guessed it right. It's not a pure function because it will return different result although we pass same input to it. In contrast, below function is a pure function:
```javascript
function someOtherFunction(num){
    return num * 2;
}
```

In pure functions, we **cannot** use:
- Random values
- current Date/Time
- read or change global state like DOM, files, db, etc.
- mutation of parameters

If you have worked on **redux**, you probably have heard that the **reducers should be pure functions.**

You probably might ask are there some examples to demonstrate the third point. Well, there are many but let me give one easy example:

```javascript
function isEligible(age){
    return age >= eligibleAge;
}
```
The above function **<code>isEligible()</code>** is not pure because it depends on **<code>eligibleAge</code>** which is not local and if we change its value the overall result from the function changes. So if we want to convert this function to a pure one, we need to pass **<code>eligibleAge</code>** to its parameter.

#### Benefits of pure function
- They're self-documenting because everything function needs and returns is clearly specified in its signature.
- They can be very easily tested.
- They can be run in parallel since they're not dependent which provides concurrency to the application thereby giving an overall performance boost.
- They're cacheable i.e. let's say the function performed some machine-intensive calculations and produced some result that might be needed quite frequently so in order to save the CPU cycles we can cache the results and reuse it(Try to recall overlapping sub-problems which mainly involves [dynamic programming](https://en.wikipedia.org/wiki/Dynamic_programming))

### Conclusion
These are some key terminologies which are key to functional programming in javascript. There is one major concept i.e. **immutability** which also is generally covered within functional programming paradigm but I'll discuss them in some other article.


{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}

