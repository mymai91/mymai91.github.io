---
layout: post
title:  "[React] State management in your React applications"
comments: true
date:   2019-07-02 15:07:33 +0700
categories: React
---

# State management in your React applications

`useState` allows you to add state to a functional component, while `useContext` allows you to share state between components without having to pass props down through every level of the component tree.

1. useState:`useState` allows you to manage state in functional component, `useState` function to update the state value.
2. useContext: allows you to share state between components without having to pass props down through every level of the component tree.
3. Third-party state management libraries:
4. Several third-party libraries can be used for state management in React, including:
- Redux: Redux is a popular library for managing application state. It uses a centralized store to manage state and allows components to subscribe to the store and receive updates as the state changes.
- MobX: MobX is a lightweight library for state management that uses observable data structures to track changes to state and automatically update components when the state changes.
- X-state

The choice of state management library depends on the specific needs of the application. For example, if the application has a large and complex state, Redux might be a good choice. On the other hand, if the state is relatively simple, the `useState`, `useContext` might be sufficient.
