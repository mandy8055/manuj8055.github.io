---
title: for..of vs for..in loops
tags: [javascript, es6, Objects, Arrays]
description: Distinction between for..of and for..in loop and related concepts in javascript
comments: true
mathjax: false
style: fill
color: info
---

## for..of vs for..in

In JavaScript, both `for..of` and `for..in` loops are used to iterate over collections. However, they are used in different scenarios.

for..in loop iterates over the **enumerable properties of an object**, including its prototype chain. It is typically used for iterating over object keys or array indexes. The syntax of the `for..in` loop is as follows:

```js
for (variable in object) {
  // code block to be executed
}
```

Here, _variable_ represents a **property name** and _object_ is the object over which _iteration is performed_. For example, the following for..in loop is used to iterate over the properties of an object:

```js
const person = {
  firstName: 'John',
  lastName: 'Doe',
  age: 25,
};

for (let property in person) {
  console.log(`${property}: ${person[property]}`);
}
/* Outputs:
firstName: John
lastName: Doe
age: 25
*/
```

In contrast, `for..of` loop is used to **iterate over iterable objects** such as arrays, strings, maps, sets, etc. It was introduced in **ES6** and provides a cleaner syntax for iterating over collections. The syntax of the `for..of` loop is as follows:

```javascript
for (variable of iterable) {
  // code block to be executed
}
```

Here, _variable_ represents an element in the iterable object, and _iterable is the iterable object being looped over_. For example, the following for..of loop is used to iterate over an array:

```javascript
const colors = ['red', 'green', 'blue'];

for (let color of colors) {
  console.log(color);
}
/* Outputs:
red
green
blue
*/
```

**NOTE:** <mark style="background-color: yellow">for..of loop cannot be used with objects as they are not iterable.</mark>

## Questions worth asking

> What Does Enumerable properties mean in simpler terms?

In JavaScript, enumerable properties of an object are properties that can be iterated over using a loop. These are properties that are "countable" or "listable" because they have a numeric property name or a string property name.

By default, _all properties that you define in an object are enumerable_, but you can set the enumerable property descriptor to false to make them non-enumerable.

> Can you give an example how will convert an enumerable property to not enumerable one?

In JavaScript, properties that are defined with `Object.defineProperty` method with `enumerable: false` flag or those that are defined on the prototype chain of an object with enumerable: false are not enumerable.

```js
const obj = {
  name: 'John',
  age: 30,
};

for (const prop in obj) {
  console.log(prop);
}
// Output: "name", "age"
---------
const obj = {};
Object.defineProperty(obj, 'prop', {
  enumerable: false,
  value: 'Hello',
});

for (const prop in obj) {
  console.log(prop);
}
// Output: nothing will be printed
```

> Can we make all the properties of an object un-enumerable. Note the object is an existing one and the properties are not known in advance?

We can use the `Object.getOwnPropertyNames()` method to get an array of all property names of an object and then set the enumerable property of each property descriptor to false using the `Object.defineProperty()` method in a loop. Here's how:

```js
const obj = {
  foo: 1,
  bar: 2,
  baz: 3,
};

// Get an array of all property names
const propNames = Object.getOwnPropertyNames(obj);

// Set enumerable to false for each property descriptor
propNames.forEach((propName) => {
  const propDesc = Object.getOwnPropertyDescriptor(obj, propName);
  Object.defineProperty(obj, propName, {
    ...propDesc,
    enumerable: false,
  });
});
```

> "Ojects are not iterable by default but Arrays are". What does this mean?

Please refer **[this blog](https://mandy8055.github.io/blog/arrays-in-js#what-are-iterables-and-array-like-objects)** for detailed explanation.

> How to find if the object/data-structure is iterable or not?

Please refer **[this blog](https://mandy8055.github.io/blog/arrays-in-js#what-are-iterables-and-array-like-objects)** for detailed explanation.

> Can we make an object iterable?

Yes, we can make an object iterable by defining a method called `Symbol.iterator` on the object. This method should **return an iterator object** with a `next method` that returns an **object with value and done properties**.

```js
const myObject = {
  a: 1,
  b: 2,
  c: 3,
  [Symbol.iterator]() {
    const keys = Object.keys(this);
    let index = 0;
    return {
      next: () => {
        if (index < keys.length) {
          return { value: this[keys[index++]], done: false };
        } else {
          return { done: true };
        }
      },
    };
  },
};

for (const value of myObject) {
  console.log(value);
}
```

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}
