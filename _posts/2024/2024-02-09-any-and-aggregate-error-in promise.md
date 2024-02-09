---
title: Promise.any Method in Javascript
tags: [javascript, es6, Promises]
description: Information around Promise.any and AggregateError method in Javascript
comments: true
mathjax: false
style: fill
color: info
---

In this post, I'll try to share my learning around _[`Promise.any`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/any)_ method and _[AggregateError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AggregateError)_ class which would be useful for understanding its usage and applying it for your specific requirement.

### What `Promise.any()` does?

`Promise.any` takes an iterable of promises and returns a new promise that is fulfilled with the value of the first fulfilled promise in the iterable. If all promises reject, `Promise.any` rejects with an `AggregateError` containing all rejection reasons.

```js
const promise1 = Promise.reject(new Error('First failed'));
const promise2 = new Promise((resolve) => setTimeout(resolve, 100, 'Success'));
const promise3 = Promise.reject(new Error('Second failed'));

Promise.any([promise1, promise2, promise3])
  .then((value) => console.log(value))
  .catch((error) => console.error(error));
// Output: "Success"
```

### Use cases of `Promise.any()`

- You need the result of the first fulfilled promise, and you can ignore rejected promises.
- You want to try multiple sources for the same data and use whichever succeeds first.

### AggregateError Class

`AggregateError` is a new error class introduced alongside `Promise.any`. It is used to represent multiple errors in a single error object. When `Promise.any` rejects, it throws an `AggregateError` containing an array of errors from all rejected promises. It is used when:

- You need to handle multiple errors together in a single catch block.
- You want to provide detailed error information when using `Promise.any`.

```js
const promise1 = Promise.reject(new Error('First failed'));
const promise2 = Promise.reject(new Error('Second failed'));
const promise3 = Promise.reject(new Error('Third failed'));

Promise.any([promise1, promise2, promise3])
  .then((value) => console.log(value))
  .catch((error) => {
    console.error('All promises rejected');
    console.error(error); // AggregateError with an array of errors
  });
// Output:
// All promises rejected
// AggregateError: All promises were rejected
```

### Practical Use-cases of `Promise.any()`

1. **Trying multiple data sources:** Suppose you have multiple sources providing the same data, and you want to use the data from the first source that succeeds. You can use `Promise.any` to get the data from the first successful source, ignoring any failed sources.

   ```js
   const fetchFromSourceA = () => fetch('https://api.sourceA.com/data');
   const fetchFromSourceB = () => fetch('https://api.sourceB.com/data');
   const fetchFromSourceC = () => fetch('https://api.sourceC.com/data');

   Promise.any([fetchFromSourceA(), fetchFromSourceB(), fetchFromSourceC()])
     .then(async (response) => {
       const data = await response.json();
       console.log('Data fetched:', data);
     })
     .catch((error) => console.error('All sources failed:', error));
   // Output: Data fetched: { ... } from the first successful source
   ```

2. **Fallback strategies:** Imagine you have a primary service and a backup service for providing data. You want to use data from the primary service, but if it fails, you want to use data from the backup service. You can use `Promise.any` to implement this fallback strategy.

   ```js
   const fetchPrimaryData = () => fetch('https://api.primary.com/data');
   const fetchBackupData = () => fetch('https://api.backup.com/data');

   Promise.any([fetchPrimaryData(), fetchBackupData()])
     .then(async (response) => {
       const data = await response.json();
       console.log('Data fetched:', data);
     })
     .catch((error) =>
       console.error('Both primary and backup sources failed:', error),
     );
   // Output: Data fetched: { ... } from the primary or backup source
   ```

3. **Optimistic UI Updates:** Suppose you have a web application that fetches data from a slow API. You want to update the UI as soon as possible using cached data, but you also want to fetch the latest data from the API. You can use `Promise.any` to update the UI with the first available data, either cached or fetched.

   ```js
   function getLocalData() {
     // Implementation of getting cached data, which returns a Promise
     return Promise.resolve({ cached: 'data' });
   }
   function getRemoteData() {
     // Implementation of fetching data from the API, which returns a Promise
     return fetch('https://api.example.com/data');
   }
   Promise.any([getLocalData(), getRemoteData()])
     .then(async (response) => {
       const data =
         typeof response.json === 'function' ? await response.json() : response;
       console.log('Data fetched:', data);
     })
     .catch((error) => console.error('Failed to get data:', error));
   // Output: Data fetched: { ... } from the local cache or the remote API
   ```

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}
