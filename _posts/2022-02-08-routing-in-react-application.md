In a React application, routing can be handled using various libraries. I used `React Router` , `Next.js` to handle it.

1. React Router: This is the most popular and widely used library for routing in React applications. It provides a declarative way to define routes, nested routes, and dynamic routes using React components.
2. Next.js: This is a popular server-side rendering framework for React that also includes built-in routing capabilities. Next.js allows you to define your routes using a file-based system and provides server-side rendering out of the box.

In terms of ***how to handle routing in a React application***, the basic steps are:

1. Define the routes for your application using the library's API.
2. Create React components for each of your routes.
3. Use the library's components and APIs to render the appropriate component for each route.

For example, with `React Router`, you would typically define your routes in a separate file and use the `<Route>` and `<Switch>` components to render the appropriate component for each route. Here's an example:

```jsx
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

function App() {
  return (
    <Router>
      <Switch>
        <Route exact path="/">
          <HomePage />
        </Route>
        <Route path="/about">
          <AboutPage />
        </Route>
        <Route path="/contact">
          <ContactPage />
        </Route>
      </Switch>
    </Router>
  );
}

```

In this example, we are defining three routes: the home page, the about page, and the contact page. We are using the `<Switch>` component to match the current URL to one of the defined routes, and then rendering the appropriate component using the `<Route>` component.
