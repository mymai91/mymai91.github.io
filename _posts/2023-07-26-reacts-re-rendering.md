---
layout: post
title:  "[React] React's re-rendering"
comments: true
date:  2023-07-26 15:07:33 +0700
categories: React
---


# React's re-rendering

React's re-rendering is driven by changes in state or props. The way it checks for changes depends on the type of values being compared.

**I) Primitive Values (Strings, Numbers, Boolean...)**

- React performs a **shallow comparison NO deep comparison** => it checks

**if** oldValue `===` newValue ***return*** **false => React will proceed with render**

**II) Objects and Arrays**

- React's default behavior does **not** involve **deep comparison** of **objects** or **arrays =>** React will **not** know to **re-render** since, because the props or state have not changed.
- **Objects** and **arrays** are reference types, meaning that even if two objects or arrays contain the same data, they are **considered** **different** because they occupy different locations in memory.
- For instance :

**{} === {} => return false**

**[] === [] => return false**

When compare **newValue === oldValue** =>  return false => component will be re-render => it might be an **unnecessary** re-rendering when you **compare {} === {} or [] === []**

**III) Handling Objects and Arrays**

- If you want
- For an object: `setState({ ...state, newProperty: value })`
- For an array: `setState([...state, newValue])`

This approach updates the state with a new reference, which React can detect, leading to a re-render.
