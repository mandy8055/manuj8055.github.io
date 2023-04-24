---
title: Reduce in Javascript
tags: [javascript, es6, Arrays]
description: Detailed explanation on reduce method of javascript arrays
comments: true
mathjax: false
style: fill
color: info
---

JavaScript’s reduce() method is a powerful and widely used function that allows developers to manipulate arrays in a concise and functional way. In this blog post, we will explore what the reduce() method is, how it works, and some of the most common use cases for this method.

## What is the reduce() Method?

The `reduce()` method is a built-in JavaScript function that takes an array and applies a function to each element in the array, resulting in a single output value. It takes two arguments: **a callback function and an optional initial value.**

The callback function takes two parameters: an accumulator and a current value. The accumulator is the result of the previous callback execution, and the current value is the value of the current element in the array. The callback function returns a new value that is then passed to the next iteration of the function, until all elements in the array have been processed.

Here is the basic syntax of the `reduce()` method:

```js
array.reduce(callback[, initialValue])
```

The _initialValue_ parameter is optional, and if it is not provided, the reduce() method will start with the first element of the array as the initial accumulator.

## How the reduce() Method Works

The reduce() method works by applying a provided function to each element in the array, resulting in a single output value. Here is the basic flow of how the reduce() method works:

1. The `reduce()` method initializes an accumulator variable with an initial value (if provided) or the first element in the array.
2. The `reduce()` method loops through the array, applying the callback function to each element in the array.
3. The callback function updates the accumulator variable with the new value returned from each iteration.
4. When all elements in the array have been processed, the final value of the accumulator is returned.

## Analogy to remember the syntax

When I first used reduce, I found the syntax quite overwhelming. There were times when I was unable to properly grasp its syntax and I believe most of the us face the similar issue. So I came up with an easy analogy to remember the syntax.

Imagine you are packing a suitcase for a trip. You have a list of items (the array) that you need to fit into the suitcase (the accumulator). Each item has a weight (the element) that you need to add up to make sure you don't exceed the weight limit (the initial value).

To do this, you start with an empty suitcase (the initial value), and then for each item in the list, you add its weight to the suitcase (the accumulator) until you've gone through the entire list.

This is similar to how the reduce method works - you start with an initial value, then for each element in the array, you perform some operation and update the accumulator until you've gone through the entire array.

## Common Use Cases for the reduce() Method

The reduce() method is a powerful and versatile function that can be used in a variety of ways. Here are some of the most common use cases for this method:

### 1. Summing an Array

The `reduce()` method can be used to sum the elements in an array. Here is an example:

```js
const numbers = [1, 2, 3, 4, 5];
const sum = numbers.reduce((accumulator, currentValue) => {
  return accumulator + currentValue;
});
console.log(sum); // Output: 15
```

In this example, the `reduce()` method initializes the accumulator variable with the first element in the array (1), and then adds each subsequent element to the accumulator variable, resulting in a final sum of 15.

### 2. Finding the maximum value in an array

The reduce() method can also be used to find the maximum value in an array. Here is an example:

```js
const numbers = [1, 5, 3, 2, 4];
const max = numbers.reduce((accumulator, currentValue) => {
  return Math.max(accumulator, currentValue);
});
console.log(max); // Output: 5
```

In this example, the reduce() method compares the accumulator variable to each element in the array and returns the maximum value.

### 3. Flattening Nested Arrays

The reduce() method can be used to flatten nested arrays into a single array. Here is an example:

```js
const nestedArray = [
  [1, 2],
  [3, 4],
  [5, 6],
];
const flatArray = nestedArray.reduce((accumulator, currentValue) => {
  return accumulator.concat(currentValue);
}, []);
console.log(flatArray); // Output: [1, 2, 3, 4, 5, 6]
``;
```

### 4. Calculating the total value of the object array

You can use reduce to calculate the total value of an object array by passing an initial value of 0 and adding each object's value to the accumulator.

```js
const orders = [
  { id: 1, value: 10 },
  { id: 2, value: 20 },
  { id: 3, value: 30 },
];

const totalValue = orders.reduce(
  (accumulator, currentValue) => accumulator + currentValue.value,
  0,
);

console.log(totalValue); // output: 60
```

### 5. Grouping array items by a property value

You can use reduce to group array items by a property value. In this example, we are grouping a list of products by their category:

```js
const products = [
  { name: 'Product 1', category: 'A' },
  { name: 'Product 2', category: 'B' },
  { name: 'Product 3', category: 'A' },
  { name: 'Product 4', category: 'C' },
  { name: 'Product 5', category: 'B' },
  { name: 'Product 6', category: 'C' },
];

const groupedProducts = products.reduce((accumulator, currentValue) => {
  const category = currentValue.category;
  if (!accumulator[category]) {
    accumulator[category] = [];
  }
  accumulator[category].push(currentValue);
  return accumulator;
}, {});

console.log(groupedProducts);

/* output:
{
  A: [{ name: 'Product 1', category: 'A' }, { name: 'Product 3', category: 'A' }],
  B: [{ name: 'Product 2', category: 'B' }, { name: 'Product 5', category: 'B' }],
  C: [{ name: 'Product 4', category: 'C' }, { name: 'Product 6', category: 'C' }]
}
*/
```

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}
