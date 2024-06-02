---
layout: post
title:  "[Redux] Redux re-render - way to prevent unnecessary re-rendering"
comments: true
date:  2023-09-18 15:07:33 +0700
categories: Redux
---


# Redux re-render - way to prevent unnecessary re-rendering

To **refuse unnecessary** re-rendering we need to use equalityFn is **shallowEqual and** some situations we might need **to** use **custom** equalityFn

When an action is dispatched, `useSelector()` will do a reference comparison of the previous selector result value and the current result value. If they are different, the component will be forced to re-render. If they are the same, the component will not re-render.

`useSelector()` will use `===` check equality by default

It will be easy if you compare **string, number, boolean**

```
'a' === 'a' => true
1 === 1 => true
false === false => true
```

**BUT** for **array or object**

```
[] === [] => false
{} === {} => false
```

because it located different in memory

which is when you get data from store, if the state you get has datatype is **object/array** you **shallowEqual** function from redux to avoid **unnecessary** re-rendering

**For example**

if the new **productPages** is same value with old **productPages => will not re-render**

```
// productPagesSelectors.selectByIds(state, ids) => return an array

export const useProductPages = (ids: EntityId[]) => {
  const productPages = useSelector(
    (state: RootState) => productPagesSelectors.selectByIds(state, ids),
   shallowEqual
  )

  return { productPages }
}

```

## **Custom equality function**

```
import { useSelector } from 'react-redux'

// equality function
const customEqual = (oldValue, newValue) => oldValue === newValue

// later
const selectedData = useSelector(selectorReturningObject, customEqual)
```

For example

When you upload image, data return will be has different status base on each process

**Process 1**

```
[
    {
        "id": "XFbqcdd",
        "name": "image_13.png",
        "type": "image/png",
        "size": 452965,
        "imageId": "p6532",
        "status": "resizing",
        "isReady": false,
        "hash": "be712903299282d5837e9dd9cd6dc18b",
        "progress": 100,
        "hasFailed": false,
        "error": null,
        "bucket": "printicular-upload-production",
        "location": "https://printicular-upload-production.s3.amazonaws.com:443/a0lvh1km3-htgw53xse-polhmddwp-zvrs28y79/be712903299282d5837e9dd9cd6dc18b"
    }
]
```

**Process 2**

```
[
    {
        "id": "XFbqcdd",
        "name": "image_13.png",
        "type": "image/png",
        "size": 452965,
        "imageId": "p6532",
        "status": "resized",
        "isReady": false,
        "hash": "be712903299282d5837e9dd9cd6dc18b",
        "progress": 100,
        "hasFailed": false,
        "error": null,
        "bucket": "printicular-upload-production",
        "location": "https://printicular-upload-production.s3.amazonaws.com:443/a0lvh1km3-htgw53xse-polhmddwp-zvrs28y79/be712903299282d5837e9dd9cd6dc18b"
    }
]
```

**Process 3**

```
[
    {
        "id": "XFbqcdd",
        "name": "image_13.png",
        "type": "image/png",
        "size": 452965,
        "imageId": "p6532",
        "status": "compressing",
        "isReady": false,
        "hash": "be712903299282d5837e9dd9cd6dc18b",
        "progress": 100,
        "hasFailed": false,
        "error": null,
        "bucket": "printicular-upload-production",
        "location": "https://printicular-upload-production.s3.amazonaws.com:443/a0lvh1km3-htgw53xse-polhmddwp-zvrs28y79/be712903299282d5837e9dd9cd6dc18b"
    }
]
```

**Final Process**

```
[
    {
        "id": "XFbqcdd",
        "name": "image_13.png",
        "type": "image/png",
        "size": 452965,
        "imageId": "p6532",
        "status": "ready",
        "isReady": true,
        "hash": "be712903299282d5837e9dd9cd6dc18b",
        "progress": 100,
        "hasFailed": false,
        "error": null,
        "bucket": "printicular-upload-production",
        "location": "https://printicular-upload-production.s3.amazonaws.com:443/a0lvh1km3-htgw53xse-polhmddwp-zvrs28y79/be712903299282d5837e9dd9cd6dc18b"
    }
]
```

Base on each process 1, 2, 3 and final, only **state** with  `status: "ready"` has **isReady:** **true,** the others status have **isReady: false, which is when isReady is false, the component should re-rendering. that why we need to custom** `customEqual`  to compare the value and force the component re-render when **isReady:** **true**

```
const customEqual = (oldValue: any, newValue: any) => {
  const val1 = oldValue.map((upload: any) => omit(upload, "status"))
  const val2 = newValue.map((upload: any) => omit(upload, "status"))

  return isEqual(val1, val2)
}

export const useUploadEntitiesWithShallowEqual = (ids: EntityId[]) => {
  const uploads = useSelector<RootState, UploadEntity[]>(state => {
    const uploadArr = uploadsSelectors.selectByIds(state, ids)
    return uploadArr
  }, customEqual)
  return { uploads }
}
```
