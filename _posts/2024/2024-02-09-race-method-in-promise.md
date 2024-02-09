---
title: Promise.race Method in Javascript
tags: [javascript, es6, Promises]
description: Information around Promise.race method in Javascript
comments: true
mathjax: false
style: fill
color: info
---

In this post, I'll try to share my learning around _[`Promise.race`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)_ method which would be useful for understanding its usage and applying it for your specific requirement.

### What `Promise.race()` does?

`Promise.race` takes an iterable of promises and returns a new promise that is settled (either fulfilled or rejected) as soon as one of the promises in the iterable is settled. The resulting promise takes on the state (fulfilled or rejected) and value of the first settled promise.

```js
const promise1 = new Promise((resolve) => setTimeout(resolve, 200, 'Slow'));
const promise2 = new Promise((resolve) => setTimeout(resolve, 100, 'Fast'));

Promise.race([promise1, promise2])
  .then((value) => console.log(value))
  .catch((error) => console.error(error));
// Output: "Fast"
```

### Use cases

- You need the result of the first completed promise, regardless of whether it's fulfilled or rejected.
- You want to implement a timeout for a promise.

### Practical Use-cases

1. **Request timeout:** Suppose you are fetching data from an API, and you want to set a timeout so that if the response takes too long, the request will be considered a failure. You can use Promise.race to race between the fetch request and a timeout promise.

   ```js
   const fetchData = () =>
     fetch('https://jsonplaceholder.typicode.com/posts/1');
   const timeout = (ms) =>
     new Promise((_, reject) =>
       setTimeout(() => reject(new Error('Request timed out')), ms),
     );

   Promise.race([fetchData(), timeout(5000)])
     .then(async (response) => {
       const data = await response.json();
       console.log('Data fetched:', data);
     })
     .catch((error) => console.error(error));
   // Output: Data fetched: { ... } or "Request timed out"
   ```

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}
