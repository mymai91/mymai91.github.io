---
layout: post
title:  "[Javascript] Sort in Javascript"
comments: true
date:   2022-08-12 15:07:33 +0700
categories: Javascript
---

I have 2 functions:

One is **executeSortUpvoted** and another one is **executeSortUpvoted2**

```
const executeSortUpvoted = (articles) => {
  return [...articles].sort((art1, art2) => {
    return art2.upvotes - art1.upvotes;
  });
};

```

```

const executeSortUpvoted2 = (articles) => {
  return articles.sort((art1, art2) => {
    return art2.upvotes - art1.upvotes;
  });
};
```

The reason why `executeSortUpvoted` **can update state** when triggered and `executeSortUpvoted2` **cannot update state** is because of the way JavaScript **handles array mutation**.

`executeSortUpvoted` uses the spread operator `[...articles]` to create a new array, which is a copy of the original `articles` array. This means that any changes made to the sorted array will not affect the original `articles` array. Therefore, when you trigger the `executeSortUpvoted` function, it returns a new sorted array, which can be used to update the state.

On the other hand, `executeSortUpvoted2` sorts the original `articles` array in place using the `sort()` method. This means that any changes made to the sorted array will also affect the original `articles` array. Therefore, when you trigger the `executeSortUpvoted2` function, it sorts the original `articles` array and returns it, but it does not create a new array. Since the original array is not changed, the state will not be updated.

**Another way is** `[...articles]` you can use `.slice()` to create a copy of array.

```
const executeSortUpvoted2 = (articles) => {
  return articles.slice().sort((art1, art2) => {
    return art2.upvotes - art1.upvotes;
  });
};
```

```
const sortByUpvotes = (articles) =>
  [...articles].sort(function (a, b) {
    const keyA = a.upvotes;
    const keyB = b.upvotes;
    if (keyA < keyB) return 1;
    if (keyA > keyB) return -1;
    return 0;
  });

const sortByLatestDate = (articles) =>
  [...articles].sort(function (a, b) {
    const keyA = new Date(a.date);
    const keyB = new Date(b.date);
    if (keyA < keyB) return 1;
    if (keyA > keyB) return -1;
    return 0;
  });
```
