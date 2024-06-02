---
layout: post
title:  "[React] From Plain HTML to React: Enhancing Interactivity with Custom Event Handling"
comments: true
date:  2024-02-25 15:07:33 +0700
categories: React
---


# From Plain HTML to React: Enhancing Interactivity with Custom Event Handling

### **1) In certain scenarios, you might use HTML from an external source and import it into your React code using the following approach:**

```
<Box
  dangerouslySetInnerHTML={{
    __html: html,
  }}
/>

```

Suppose you want an **onclick function** within this HTML to **trigger** functionality within your **React** code, such as **Redux actions or custom hooks**. **==> How can you achieve this?**

### **Define component**

Define a `PublicApi.tsx` component.

We need to check `isBrowser` because the compile time will differ

```
export const isBrowser = typeof window !== "undefined"
```

```
import React from "react";
import { useApplicationState } from "~/hooks/useApplicationState";
import { useLineItemActions } from "~/hooks/useLineItem";

interface Props {}

const PublicApi: React.FC<Props> = () => {
  const fullState = useApplicationState();

  const { insertPageSpreadAfter } = useLineItemActions();

  if (isBrowser) {
    window.sayHello = () => {
      alert("Hello Browser");
    };
    window.reduxIt = () => {
      console.log(insertPageSpreadAfter);
      insertPageSpreadAfter(1, 1);
    };
  }

  return null;
};

export default PublicApi;

```

Then, open `App.tsx` and import the `PublicApi` component.

```
import { Provider } from "react-redux";
import "./App.css";
import { Home } from "./components/Home";
import { PublicApi } from "./components/PublicApi";
import store from "./redux/store";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Provider store={store}>
        <Home />
        <PublicApi />
      </Provider>
    </QueryClientProvider>
  );
}

export default App;

```

### **Add typescript configure**

If you are using TypeScript, open `module.d.ts` and add definitions to make TypeScript recognize the new functions.

```
declare module "*.svg" {
  export const ReactComponent: any;
}

interface Window {
  sayHello: () => void;
  reduxIt: () => void;
}
declare module "*.scss" {
  const content: { [className: string]: string };
  export = content;
}

```

### **To test it, open your page, inspect the element, go to the console tab, and check:**

```
window.sayHello();
```
