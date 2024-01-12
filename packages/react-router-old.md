# React Router

## Creating Routes the old way

- Import into app/main:

```js
import { BrowserRouter, Routes, Route } from "react-router-dom";
```

- The BrowserRouter, Routes, and Route component functions imported must all be used to tell React how to use our routes:

```js
const App = () => {
    return (
        <BrowserRouter>
        <Routes>
            <Route path='/' element={<Homepage />} />
            <Route path='about' element={<About />} />
            <Route path='contact' element={Contact />} />
            <Route path='*' element={PageNotFound />} />
        </Routes>
        </BrowserRouter>
    )
}
```

- Each route must have a path and the element that should be loaded:

```html
<Route path="about" element="{<About" />} />
```

- The element is set up in the same way as a regular React component function:

```js
// Homepage.jsx
const Homepage = () => {
    return (
        // some stuff
    )
}
export default Homepage;
```

- It is convention to create a `pages` directory within the `src` directory to store these elements/pages in.

### Nested Routing

- A nested route is information in our path in addition to the original route.

- For example, `www.mypage.com/app` -> `www.mypage.com/app/nested`.

- To achieve this, pass the nested routes in as children to our main routes.

```jsx
<Route path="app" element={<App />}>
  <Route path="nested" element={<Nested />} />
  <Route path="todolist" element={<TodoList />} />
</Route>
```

- To render the correct components, we then use `Outlet`.

- Instead we import and use:

```js
// start of file
import { Outlet } from "react-router-dom";

// where we want the compnent(s) within the layout
<Outlet />;
```

- This is similar to using `{children}` as we would to display children props.

### Index Route

- If a route by default does not have it's own content, we can set a defult nesting to display.
- Done with the index keyword within the route.
- In the example below, whenever the app is loaded, the TodoList component will be rendered.

```jsx
<Route path="app" element={<App />}>
  <Route index element={<TodoList />} />
  <Route path="nested" element={<Nested />} />
  <Route path="todolist" element={<TodoList />} />
</Route>
```
