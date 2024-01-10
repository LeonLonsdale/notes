# React Router

## Installation

- React Router doesn't come with React so it has to be downloaded separately:

```
npm i react-router-dom
```

## Creating Route

- To view the old way (pre v6) check [Routing the old way](./react-router-old.md).

- Import `RouterProvider` and `createBrowserRouter` from `react-router-dom`

```ts
import { RouterProvider, createBrowserRouter } from "react-router-dom";
```

- Do this in the app/main file.
- Create the router

```ts
const router = createBrowserRouter();
```

- `createBrowserRouter()` acccepts an array of route objects.
- Each route object should contain a path and an element.

```ts
const router = createBrowserRouter([{ path: "/", element: <Home /> }]);
```

### Using the router

- Simply return the RouterProvider, passing in the router as a prop:

```ts
import { RouterProvider, createBrowserRouter } from "react-router-dom";

const router = createBrowserRouter([{ path: "/", element: <Home /> }]);

export default function App() {
  return <RouterProvider router={router} />;
}
```

### Nesting Routes

- To nest routes, we can also provide a `children` property to the route object.
- This property accepts an array of child routes.

```ts
const router = createBrowserRouter([
  {
    element: <Layout />,
    children: [{ path: "/", element: <Home /> }],
  },
]);
```

### Rendering Nested Routes

- Rendering a nested route is done in the same way as the old method, using `Outlet`.

```ts
// Layout.tsx
import { Outlet } from 'react-router-dom';

export default function Layout() {
  return (
    <Header />
    <main>
      <Outlet />
    </main>
    <Footer />
  )
}
```

## Linking Between Pages

A simple anchor tag would trigger a new page request which is not what we want in a single page application. React Router provides us with a `Link` element to create links.

It needs to be imported:

```js
import { Link } from "react-router-dom";
```

It can then be used to create a link. It accepts a `to` prop, which is the route we want it to point at.

```html
<Link to='/store'>Store</Link>
```

[Back to Contents](./README.md) - [Back to Top](#)

## NavLink

React Router also provides us with a `NavLink` element. This is specifically for Navigation lists, and provides additional features such as applying an active class to the currently active page.

It works in the same way as `Link`.

[Back to Contents](./README.md) - [Back to Top](#)

## Storing State in the URL

The URL is a great place to store UI state and an alternative to useState in some situations.

Some benfits include:

- You can share the URL with others, and they will render the same view. For exampel if you've selected the size and colour of a T-Shirt and want to share it with a friend.
- You can bookmark the URL to return to it later without losing progress.
- The state is available to all components without having to use prop drilling.
- The data can easily be passed to the next page.

We can store state in the URL using `params` and the `query string`

```
www.myapp.com/app/products/1381239216391?size=l&colour=black

param = 1381239216391 // would be replaced with an actual username.
query string = ?size=l&colour=black
```

We would need links set up that include the details we want to store/pass along:

```html
<Link to=`${productId}?size=${size}&colour=${colour}`> ... </Link>
```

### Params

To achieve this, we first give our params an alias within a route:

```html
<Route path="products/:id" element="{...}" />
<!-- :username here is the alias -->
```

We can then, in our component, access this alias by using the `useParams()` hook provided by React Router

```js
import { useParams } from "react-reouter-dom";

const User = () => {
  const params = useParams();
  // params will be an object of all the params
  // there can be multiple under different alias's in the route
  // the object properties will take the name of the alias's provided
  // { id: 1381239216391 }
  // so this could be destructured from the start
  // const { id } = useParams();
};
```

### Query String

React Router also provides us with a hook for the search query. This works similar to useState in the syntax. It gives us a variable and a function.

The values aren not immediately accessible, so the variable we receive back comes with a get property that we use.

```js
import useSearchParams from "react-router-dom";

const User = () => {
  const [searchParams, setSearchParams] = useSearchParams();
  const size = searchParams.get("size");
  const colour = searchParams.get("colour");
};
```

We can then change the search params with the function we now have available. For example, if the user wanted to take a look at a white T shirt instead of a black T shirt:

```js
onClick={() => setSearchParams({ size: 'l', colour: 'white' })};
```

## Programatic Navigation

### Imperative with the `useNavigate()` Hook

Sometimes we want certain actions to take the user to a specific route, but those actions may not necessarily involve clicking a link or a button - for exaple: form submission, clicking a location on a map.

React Router provides us with `useNavigate()` for just this purpose. This returns a navigate function into which we pass the url parameter as an arguament.

```js
import { useNavigate } from "react-router-dom";

const MyComponent = () => {
  const navigate = useNavigate();
  return <div onClick={() => navigate("param")}>// ...</div>;
};
```

Additionally, we may occasionally want to include back or forward buttons. This is simply achieved by passing in the number of steps forward or backward we want the button to take us. Negative numbers for back, and positive numbers for forward.

```js
navigate(-1); // back 1
navigate(1); // forward 1
```

### Declarative with the `<Navigate />` component

Whenever the hook cannot be properly used, we can use this more declarative option. This is particularly useful inside nested routes when using default index routes. In [Index Route](#index-route) we used the following example:

```jsx
<Route path="app" element={<App />}>
  <Route index element={<TodoList />} />
  <Route path="nested" element={<Nested />} />
  <Route path="todolist" element={<TodoList />} />
</Route>
```

In this example, loading `www.mydomain.com/app` will load the default index child route component `<TodoList />`. The address in the address bar, however, will not update to `app/todolist` it will still just be `/app`.

To fix this we tell the index to use the `<Navigate />` component which, similar to Link and NavLink, accepts a `to` property.

```jsx
<Route index element={<Navigate to="todolist" />} />
```

This essentially then re-directs the browser to `app/todolist` and React Router will then follow any instructions we've provided as to how to handle that route.

One issue with this is that once this re-direct has happened, we're unable to use the back button. So we add the `replace` keyword into the Navigate component.

```jsx
<Route index element={<Navigate replace to="todolist" />} />
```

## Data Fetching

- This is not possible using the older method of creating routes.
- First we create a loader - by convention, do this in the file for the component that needs the data:

```ts
export async function loader() {
  const response = await fetch("url");
  if (!response.ok) throw new Error("Failed to get data");
  const { data } = await response.json();
  return data;
}
```

- Secondly, we provide the loader function to the route

```ts
// app.tsx / main.tsx

import Component, { loader as componentLoader } from "./Component";

const router = createBrowserRouter([
  {
    element: <Layout />,
    children: [
      {
        path: "/component",
        element: <Component />,
        loader: componentLoader,
      },
    ],
  },
]);
```

- Finally, we access the data within the component using the `useLoaderData()` hook.

```ts
import { useLoaderData } from 'react-router-dom';

export default function Component () {
  const data = useLoaderData();

  return (
    // JSX
  )
}


export async function loader() {
  const response = await fetch("url");
  if (!response.ok) throw new Error("Failed to get data");
  const { data } = await response.json();
  return data;
}
```
