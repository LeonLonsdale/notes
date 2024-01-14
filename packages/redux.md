# Redux

## Overview

- A tool used for managing global ui state.
- Allows us to inject state exactly where it's needed, minimising the number of components that need to be re-rendered.
- When the global store is updated, all components that consume data from that store will be re-rendered.

There are two versions:

- Classic Redux
- Redux Toolkit (more modern)

## When should we use Redux?

It's a good idea to use Redux (or similar) when dealing with a lot of Global UI state that needs to be frequently updated.

## How does Redux Work?

1. An event handler in a component calls an action creator - this writes the actions
2. The actions are passed to a dispatch
3. The dispatch passes on our action to the Store
4. The store contains multiple reducer functions, and the current state
5. Thew new state is output
6. The applicable components re-redner.

\* Each reducer is a pure function that calculates the next state (state transition) based on the action and the current state. There should be one reducer for each application feature. For example; shopping cart, user data, themes.

## Installation

```js
npm i redux // redux bas package
npm i react-redux // used to connect react & redux
npm i @reduxjs/toolkit // modern redux
```

## File Structures

It's common practice to keep redux stores in a separate file. For example, store.js.

The initial state, logic (reducers), and action creators are typically then also contained within their own `slice` files which are stored within feature directories.

For example:

```
root
  features
    customers
      customerSlice.js
    accounts
      accountSlice.js
```

# Redux Toolkit

### Create a Slice

- Import the `createSlice()` function from redux toolkit.

```ts
import { createSlice } from `@reduxjs/toolkit`
```

- Use the function and store its output.
- It accepts an object as an argument, with properties `name`, `initialState`, and `reducers`.
- The `reducers` property should be an object containing functions that will be used to create action creators.
- Reducer functions gain access to `state` and `action`.

```ts
const mySlice = createSlice({
  name: "mySliceName",
  initialState,
  reducers: {
    myFunc(state, action) {
      state.property = action.payload;
    },
  },
});
```

- mySlice is now an object containing 2 properties, `actions` and `reducer`.
- `actions` contains action creator functions that we can use throughout our codebase.
- `reducer` is the main reducer function that we use when creating our store.
- We export these from our slice file.

```ts
export const { myFunc } = mySlice.actions;
export default mySlice.reducer;
```

- By default, the action creators output from this will provide the action type named according to convention.
- In the case of this example, the action type would be `'mySliceName/myFunc'`.
- A slice named `users` may have multiple actions such as `updateUser`,`deleteUser`, `addUser`
- Such a slice would output multiple action creators

```ts
export const { updateUser, deleteUser, addUser } = userSlice.actions;
```

- These action creators would automatically use the action types: `users/addUser`, `users/deleteUser`, and `users/updateUser`.

### Access actions within actions

- When writing the functions within the reducers property of our slice, we may need to access other functions.
- For example, an action for decreasing the quantity of an item will eventually reach 0. At that point we may want to delete the item entirely - access the delete action.
- We do this will `mySlice.caseReducers` which gives us access to the other functions.

```ts
decreaseQuantity(state, action) {
  const item = state.cart.find((item) => item.id === action.payload);
  if (!item) return;
  item.quantity--;

  if (item.quantity === 0) mySlice.caseReducers.deleteItem(state, action);
}
```

### Create the store with toolkit

- Typically done in a file at the src directory level: `store.ts` / `store.js`
- Import configureStore() from @reduxjs/toolkit.

```ts
import { configureStore } from "@reduxjs/toolkit";
```

- Create a store using the function and passing in the reducer(s)

```ts
import { configureStore } from "@reduxjs/toolkit";

import userReducer from "./features/users/userSlice";

const store = configureStore({
  reducers: {
    user: userReducer,
  },
});

export default store;
```

### Connecting the store to the app

- The connection is made at the top most level of the application: `app.tsx` / `main.tsx`
- We import a provider component from react-redux, and our store

```ts
import { Provider } from "react-redux";
import store from "./store.ts";
```

- We then wrap the app in the provider, which receives the store as a prop.

```ts
ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

### Typing State and Store

- For each state we can type as normal.
- The store type then becomes an object of state types.

```ts
type UserState = {
  username: string;
};

type StoreState = {
  user: UserState;
};
```

### Accessing State

- To access state we import the `useSelector` hook into the component that needs the state.
- This hook gains access to the state object.
- The state object holds a property for each `slice` created and added to it
- Each of these properties holds the state relevant to that slice.
- this is accessed in the format: `state.slicename.dataname`

```ts
// the slice

const initialState = {
  username: "Joe Bloggs",
  age: 101,
};

const userSlice = createSlice({
  name: "user",
  initialState,
  reducers: {
    updateName(state, action) {
      state.username = action.payload;
    },
  },
});

// in the component
import { useSelector } from "react-redux";

export default function Username() {
  const username = useSelector((state) => state.user.username);

  return <p>{username}</p>;
}
```

### useSelector and selector functions

- In reality, what the `useSelector` hook receives is actually known as a `selector function`
- The recommendation is that these functions are written and exported from the slice file.
  - This means we have only a single place to edit if we change the shape of our state.
- It is a naming convention to start selector function names with `get`.
- For example:

```ts
// in slice file
const getUsername = (state) => state.user.username;

// component file that needs access to the username
const username = useSelector(getUsername);
```

- Also true of more complex selector functions:

```ts
// cart slice file

const getTotalItemsInCart = (state) =>
  state.cart.cart.reduce((sum, item) => sum + item.quantity);
const getTotalPriceOfCart = (state) =>
  state.cart.cart.reduce((sum, item) => sum + item.unitPrice * item.quantity);

// in component file that needs access to the information

const cartNumItems = useSelector(getTotalItemsInCart);
const cartTotalPrice = useSelector(getTotalPriceOfCart);
```

- If, when calling the selector function we need to pass in an argument, our selector function should become a higher-order function: it should return a function.
- In the example below:
  - We need to get the quantity value from within the item object
  - We therefore need to pass in the id of the item needed.
  - With the id passed in, it returns the selector function which now has access to the id via closure.
  - The returned function is `(state) =>
state.cart.cart.find((item) => item.id === id).quantity ?? 0`

```ts
const getNumCurrentItem = (id) => (state) =>
  state.cart.cart.find((item) => item.id === id).quantity ?? 0;

// when calling:

const numCurrentItem = useSelector(getNumCurrentItem(id));
```

### Manipulating State

- We can manipulate state using a combination of `dispatch` and our `action creators`.
- Dispatch is created using the react-redux hook `useDispatch()`
- The hook returns a dispatch function that we can store.
- We pass our action creator in, along with the intended payload, as an argument.

```ts
import { useState } from "react";
import { useDispatch } from "react-redux";

export default function MyComponent() {
  const [name, setName] = useState();
  const dispatch = useDispatch();

  const handleUpdateName = () => {
    dispatch(updateName(name));
  };

  return (
    <form>
      <input
        type="text"
        onChange={(e) => setName(e.target.value)}
        value={name}
      />
      <button onClick={handleUpdateName}>Update name</button>
    </form>
  );
}
```

# Redux Thunk

- A middleware that sits between the dispatch and the reducer
- Does something with the dispatched action before it reaches the store
- Most commonly used for async actions.
- `createAsyncThunk()` ->
  - Accepts action type as first arg
  - Accepts async fn as second arg which outputs the payload

```ts
export const myThunk = createAsyncThunk("user/fetchAddress", async () => {
  // do stuff
  return { payload };
});
```

- Produces 3 additional actions types for: pending, resolved, rejected (promise)
- Handle these additional action types in the `slice` under the property `extraReducers`.
- `extraReducers` is a function that has access to a `builder` object by default.
- The `builder` object exposes an `addCase` function which accepts the action type and an action function.
- `addCase` can be chained.

```ts
export const userSlice = createSlice({
    name: 'user',
    initialState,
    reducers: {
        action(state, action) { /* do stuff */ },
    },
    extraReducers (builder) => builder
        .addCase(myThunk.pending, (state, action) { /* do stuff */ })
        .addCase(myThunk.fulfilled, (state, action) { /* do stuff */ })
        .addCase(myThunk.rejected, (state, action) { /* do stuff */ })
});
```

- These are dispatched in the same way as other actions. However, there may be type problems with TypeScript
- By default, our dispatch function expects an arg of type `UnknownAction` but the thunk is type `AsyncThunkAction`
- To avoid this we can create `useAppDispatch` and export from ouore store file:

```ts
// from your store.ts file
export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;

// from hooks.ts file
import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";
import type { RootState, AppDispatch } from "../store";

type DispatchFunc = () => AppDispatch;
export const useAppDispatch: DispatchFunc = useDispatch;
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```
