---
title: Promise.allSettled Method in Javascript
tags: [javascript, es6, Promises]
description: Information around Promise.allSettled method in Javascript
comments: true
mathjax: false
style: fill
color: info
---

In this post, I'll try to share my learning around _[Promise.allSettled](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)_ method which would be useful for understanding its usage and applying it for your specific requirement.

### What `Promise.allSettled()` does?

`Promise.allSettled` takes an iterable of promises and returns a new promise that is fulfilled with an array of objects, each describing the outcome of each promise (either fulfilled or rejected). Unlike `Promise.all`, `Promise.allSettled` does not reject if one of the promises rejects.

```js
const promise1 = Promise.resolve('Hello');
const promise2 = Promise.reject(new Error('Something went wrong'));
const promise3 = new Promise((resolve) => setTimeout(resolve, 100, 'World'));

Promise.allSettled([promise1, promise2, promise3])
  .then((results) => console.log(results))
  .catch((error) => console.error(error));
// Output: [
//   {status: "fulfilled", value: "Hello"},
//   {status: "rejected", reason: Error: Something went wrong},
//   {status: "fulfilled", value: "World"}
// ]
```

### Use cases

- You need to wait for multiple promises to complete, regardless of whether they are fulfilled or rejected.
- You need to know the status of all promises to handle different outcomes.

### Practical Use-cases

1. **Error-tolerant data fetching:** Suppose you have an application that fetches data from multiple APIs, and you want to display partial data even if some of the API calls fail. You can use `Promise.allSettled` to handle successful and failed requests separately.

   ```js
   const fetchPosts = () => fetch('https://jsonplaceholder.typicode.com/posts');
   const fetchUsers = () => fetch('https://jsonplaceholder.typicode.com/users');
   const fetchComments = () =>
     fetch('https://jsonplaceholder.typicode.com/comments');

   Promise.allSettled([fetchPosts(), fetchUsers(), fetchComments()]).then(
     async (results) => {
       results.forEach(async (result, index) => {
         if (result.status === 'fulfilled') {
           const data = await result.value.json();
           console.log('Data fetched:', data);
         } else {
           console.error(
             `Error fetching data from API ${index}:`,
             result.reason,
           );
         }
       });
     },
   );
   ```

2. **Batch processing with error handling:** Imagine you are processing a batch of tasks, and you want to continue processing even if some tasks fail. You can use `Promise.allSettled` to handle successful and failed tasks separately.

   ```js
   function processTask(task) {
     // Implementation of task processing, which may return a Promise
   }
   const tasks = ['task1', 'task2', 'task3', 'task4'];
   const taskPromises = tasks.map(processTask);
   Promise.allSettled(taskPromises).then((results) => {
     results.forEach((result, index) => {
       if (result.status === 'fulfilled') {
         console.log(`Task ${index} completed successfully:`, result.value);
       } else {
         console.error(`Error processing task ${index}:`, result.reason);
       }
     });
   });
   ```

3. **Running multiple animations in parallel:** Suppose you have an application that performs several animations in parallel, and you want to continue the remaining animations even if some animations fail. You can use `Promise.allSettled` to handle successful and failed animations separately.

   ```js
   function runAnimation(animation) {
     // Implementation of animation, which may return a Promise
   }
   const animations = ['animation1', 'animation2', 'animation3'];
   const animationPromises = animations.map(runAnimation);
   Promise.allSettled(animationPromises).then((results) => {
     results.forEach((result, index) => {
       if (result.status === 'fulfilled') {
         console.log(`Animation ${index} completed successfully`);
       } else {
         console.error(`Error running animation ${index}:`, result.reason);
       }
     });
   });
   ```

### Polyfill for `Promise.allSettled()`

`Promise.allSettled()` is supported in modern browsers and Node.js, but it might not be available in older environments. By creating a polyfill, we ensure that our code can run in environments that do not natively support `Promise.allSettled()`.

#### Implementation steps:

1. Define the function that takes an array of promises.
2. Create an executor function to handle promise resolution and rejection.
3. Resolve the case when the input array is empty.
4. Iterate through the input promises and handle their resolved values.
5. Return a new promise that will always resolve with an array of resolved and rejected promises based on input array.

```js
function allSettled(promises) {
  const executorFunction = (resolve, _) => {
    const result = [];
    let pendingCount = promises.length;
    // base case
    if (pendingCount === 0) {
      resolve(result);
      return;
    }
    promises.forEach((promise, idx) => {
      Promise.resolve(promise)
        .then((value) => (result[idx] = { status: 'fulfilled', value }))
        .catch((reason) => (result[idx] = { status: 'rejected', reason }))
        .finally(() => {
          if (--pendingCount === 0) {
            resolve(result);
          }
        });
    });
  };
  return new Promise(executorFunction);
}
```

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}
