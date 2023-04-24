---
title: Arrays in Javascript
tags: [javascript, es6, Arrays, Iterables]
description: Introduction to arrays in javascript and various operations and methods attached to it
comments: true
mathjax: false
style: fill
color: info
---

## Arrays in javascript

An array is a collection of values that are stored in contiguous memory locations. An array can hold values of any data type, including other arrays, objects, and functions. Each element in an array is identified by an index, _starting from zero_ for the first element.

## What are Iterables and Array-like objects

An **iterable object** is any object that has a `[Symbol.iterator]` property, which **returns an iterator object.** The iterator object must have a `next()` method that returns an object with `value` and `done` properties. e.g. Arrays, Strings, Sets, Maps, Generators.

You can check if an object is iterable or not by using the `Symbol.iterator` property. If an object has a `Symbol.iterator` property, then it is iterable.

```js
const array = [1, 2, 3];
console.log(typeof array[Symbol.iterator]); // "function", so it is iterable

const obj = { a: 1, b: 2, c: 3 };
console.log(typeof obj[Symbol.iterator]); // "undefined", so it is not iterable
```

An **array-like object** is an object that has a **length property and contains elements with numeric indices, but is not actually an array**. Examples of array-like objects include the arguments object and DOM collections like document.getElementsByTagName().

Here are some key differences between iterable and array-like objects:

- Iterable objects have a `Symbol.iterator` method that returns an iterator object, while array-like objects do not have this method.
- Iterable objects can be iterated over using a `for...of` loop or the `Array.from()` method, while array-like objects can be iterated over using a for loop or converted to an array using the `Array.from()` method.
- Iterable objects can be used with other iterable-specific methods, such as the `entries()` method, which returns an iterator over the [index, value] pairs of the iterable object. Array-like objects do not have these methods.
- Iterable objects can be created using the **generator function**, which allows for more flexible and customized iteration. Array-like objects are typically created by other functions or methods, such as querySelectorAll().

Some of the objects which are both Iterator and Array-like are **NodeList**, **HTMLCollection**, **TypedArray** and **String**.

## Ways of Creating an Array

There are various ways of creating an Array in javascript. Let's look at some of the most common ways to do so.

- **Using square brackets notation**: This is the most common way to create an array in JavaScript. It involves enclosing a list of elements in square brackets.

```js
let myArray = [1, 2, 3, 4, 5];
```

- **Using the Array constructor**: You can also create an array using the Array constructor, which takes a comma-separated list of values as arguments. The point to note here if you just provide one argument as number to the constructor function, then it considers it as the length of the array and not the element.

```js
let myArray = new Array(1, 2, 3, 4, 5);
let myArray2 = new Array(5); // An empty array that has total length of 5.
```

- **Using the Array.from() method**: This method creates a new array from an _iterable object_ (as discussed above), such as an array-like object or a string.

```js
let myArray = Array.from('hello'); // ['h', 'e', 'l', 'l', 'o']
```

- **Using the Array.of() method**: This creates a new array with the specified number of elements, each initialized with the provided value.

```js
let myArray = Array.of(1, 2, 3, 4, 5);
```

## Adding and Removing elements from an array

#### Adding Elements

- **push() method**: Adds one or more elements to the end of an array.

```js
let arr = [1, 2, 3];
arr.push(4); // arr is now [1, 2, 3, 4]
```

- **unshift() method**: Adds one or more elements to the beginning of an array.

```js
let arr = [1, 2, 3];
arr.unshift(0); // arr is now [0, 1, 2, 3]
```

- **splice() method**: Adds or removes elements from an array at a specified index. **Performs inplace updations**.

```js
let arr = [1, 2, 3];
arr.splice(1, 0, 4); // arr is now [1, 4, 2, 3]
```

In the above example, `splice()` adds the value 4 at index 1 and removes 0 elements.

- **concat() method**: Returns a new array that combines two or more arrays.

```js
let arr1 = [1, 2];
let arr2 = [3, 4];
let arr3 = arr1.concat(arr2); // arr3 is now [1, 2, 3, 4]
```

#### Removing Elements

- **pop() method**: Removes the last element of an array and returns it.

```js
let arr = [1, 2, 3];
let removedElement = arr.pop(); // arr is now [1, 2], removedElement is 3
```

- **shift() method**: Removes the first element of an array and returns it.

```js
let arr = [1, 2, 3];
let removedElement = arr.shift(); // arr is now [2, 3], removedElement is 1
```

- **splice() method**: Removes elements from an array at a specified index.

```js
let arr = [1, 2, 3];
arr.splice(1, 1); // arr is now [1, 3]
```

In the above example, `splice()` removes 1 element at index 1.

- **slice() method**: Returns a new array that contains a portion of the original array.

```js
let arr = [1, 2, 3, 4, 5];
let newArray = arr.slice(1, 4); // newArray is [2, 3, 4]
```

In the above example, `slice()` returns a new array that contains elements from index 1 to index 4 (excluding index 4). The original array is not modified.

#### Finding a value in an array

- **indexOf() method**: Returns the index of the first occurrence of a specified element in an array, or -1 if the element is not found.

```js
let arr = ['apple', 'banana', 'orange'];
let index = arr.indexOf('banana'); // index is 1
```

- **lastIndexOf() method**: Returns the index of the last occurrence of a specified element in an array, or -1 if the element is not found.

```js
let arr = ['apple', 'banana', 'orange', 'banana'];
let index = arr.lastIndexOf('banana'); // index is 3
```

- **find() method**: Returns the value of the first element in an array that satisfies a specified condition, or undefined if no such element is found.

```js
let arr = [1, 2, 3, 4];
let result = arr.find((element) => element > 2); // result is 3
```

- **filter() method**: Returns a new array containing all elements in an array that satisfy a specified condition.

```js
let arr = [1, 2, 3, 4];
let filteredArray = arr.filter((element) => element > 2); // filteredArray is [3, 4]
```

- **includes() method**: Returns true if an array contains a specified element, and false otherwise.

```js
let arr = ['apple', 'banana', 'orange'];
let result = arr.includes('banana'); // result is true
```

**NOTE:** `indexOf()` and `lastIndexOf()` methods doesn't work well with reference types like object.

```js
const personData = [{ name: 'John' }, { name: 'Doe' }];
console.log(personData.indexOf({ name: 'Doe' })); // returns -1
// We can use find instead
console.log(personData.find((person) => person.name === 'Doe')); // returns {name: 'Doe'}
```

## Sorting and Reversing Arrays

The `sort()` method is used to sort the elements of an _array in place_, i.e., it modifies the original array. The default sort order is based **on the string Unicode code points**. However, you can also specify a custom sort function as an argument to the sort() method.

```js
let arr = [3, 1, 4, 2];
arr.sort(); // arr is [1, 2, 3, 4]

let arr2 = ['apple', 'Orange', 'banana', 'Orange'];
arr2.sort(); // arr2 is ['Orange', 'Orange', 'apple', 'banana']

let arr3 = [3, 1, 4, 2];
arr3.sort((a, b) => b - a); // arr3 is [4, 3, 2, 1]
```

**Good to Know:** `sort()` function uses **[tim sort](https://www.geeksforgeeks.org/timsort/)** alogrithm in backend to sort the elements.

The `reverse()` method is used to reverse the order of the elements of an _array in place_, i.e., it modifies the original array.

```js
let arr = [1, 2, 5, 3];
arr.reverse(); // arr is [3, 5, 2, 1]
```

## map, forEach, reduce methods

- **map() function**: The map function creates a new array by applying a given function to each element in the original array. The resulting array will have the same length as the original array, but each element will have been transformed in some way.

```js
const numbers = [1, 2, 3, 4];
const doubled = numbers.map((num) => num * 2);
console.log(doubled); // [2, 4, 6, 8]
```

- **forEach() function**: The forEach function allows you to iterate over each element in an array and perform some action on it. This function does not create a new array; it simply executes a function for each element in the original array.

```js
const names = ['Alice', 'Bob', 'Charlie'];
names.forEach((name) => console.log(name));
```

I've covered reduce method in **[this blog post](https://mandy8055.github.io/blog/reduce-in-js)** since it require greater attention.

## The Spread Operator and Array destructuring

#### Spread Operator

The Spread operator in JavaScript is denoted by three dots `...`. It allows an iterable (like an array or a string) to be expanded into individual elements. The Spread operator can be used in multiple ways:

- **Expanding an array into individual elements**:

```js
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

const combinedArr = [...arr1, ...arr2];

console.log(combinedArr); // [1, 2, 3, 4, 5, 6]
```

- **Passing an array as function arguments**:

```js
const arr = [1, 2, 3];

function sum(a, b, c) {
  return a + b + c;
}

const result = sum(...arr);

console.log(result); // 6
```

- **Copying an array**:

```js
const arr1 = [1, 2, 3];
const arr2 = [...arr1];

console.log(arr2); // [1, 2, 3]
```

#### Array Destructuring

Array Destructuring is a way to unpack values from an array into separate variables. It allows you to assign array elements to variables in a concise and readable way. The syntax for Array Destructuring is also denoted by square brackets [].

```js
const arr = [1, 2, 3];

const [a, b, c] = arr;

console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
```

> Array Destructuring also allows you to set default values for variables

```js
const arr = [1];

const [a, b = 2, c = 3] = arr;

console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
```

> Array Destructuring can also be used with the Rest parameter to capture the remaining elements of an array

```js
const arr = [1, 2, 3, 4, 5];

const [a, b, ...rest] = arr;

console.log(a); // 1
console.log(b); // 2
console.log(rest); // [3, 4, 5]
```

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}
