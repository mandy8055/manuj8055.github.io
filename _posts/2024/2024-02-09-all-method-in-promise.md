---
title: Promise.all Method in Javascript
tags: [javascript, es6, Promises]
description: Information around Promise.all method in Javascript
comments: true
mathjax: false
style: fill
color: info
---

In this post, I'll try to share my learning around _[`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)_ method which would be useful for understanding its usage and applying it for your specific requirement.

### What `Promise.all()` does?

`Promise.all` takes an iterable (e.g., an array) of promises and returns a new promise that is fulfilled with an array of fulfilled values, in the same order as the promises in the iterable. If any of the passed promises rejects, `Promise.all` immediately rejects with the reason of the first rejected promise.

```js
const promise1 = Promise.resolve('Hello');
const promise2 = Promise.resolve(42);
const promise3 = new Promise((resolve) => setTimeout(resolve, 100, 'World'));

Promise.all([promise1, promise2, promise3])
  .then((values) => console.log(values))
  .catch((error) => console.error(error));
// Output: ["Hello", 42, "World"]
```

### Use cases

- You need to wait for multiple promises to resolve before executing some code.
- The order of the results is important.

### Practical Use-cases

1. **Fetching multiple resources:** Suppose you are building a dashboard that requires data from multiple API endpoints. You can use `Promise.all` to fetch all the data in parallel and render the dashboard once all requests are completed.

   ```js
   const fetchPosts = () => fetch('https://jsonplaceholder.typicode.com/posts');
   const fetchUsers = () => fetch('https://jsonplaceholder.typicode.com/users');
   const fetchComments = () =>
     fetch('https://jsonplaceholder.typicode.com/comments');

   Promise.all([fetchPosts(), fetchUsers(), fetchComments()])
     .then(async ([postsRes, usersRes, commentsRes]) => {
       const posts = await postsRes.json();
       const users = await usersRes.json();
       const comments = await commentsRes.json();
       // Render the dashboard with fetched data
     })
     .catch((error) => console.error('Error fetching data:', error));
   ```

2. **Uploading multiple files:** Imagine you are building a file storage application that allows users to upload multiple files simultaneously. You can use `Promise.all` to wait until all files are uploaded before showing a success message.

   ```js
   function uploadFile(file) {
     // Implementation of file upload using fetch or other methods
   }
   const files = Array.from(document.getElementById('fileInput').files);
   const uploadPromises = files.map(uploadFile);
   Promise.all(uploadPromises)
     .then(() => {
       console.log('All files uploaded successfully');
     })
     .catch((error) => {
       console.error('Error uploading files:', error);
     });
   ```

3. **Performing batch database operations:** Suppose you need to update multiple records in a database. You can use `Promise.all` to wait for all update operations to complete before proceeding.

   ```js
   const updateRecord = (record) => {
     // Implementation of database update operation
   };

   const recordsToUpdate = [
     { id: 1, value: 'newValue1' },
     { id: 2, value: 'newValue2' },
     { id: 3, value: 'newValue3' },
   ];

   const updatePromises = recordsToUpdate.map((record) => updateRecord(record));

   Promise.all(updatePromises)
     .then(() => {
       console.log('All records updated successfully');
     })
     .catch((error) => {
       console.error('Error updating records:', error);
     });
   ```

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}
