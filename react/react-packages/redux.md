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

### Initial State & Reducers

Initial State and Reducer functions are set up in the very same way as they would be with useReducer. The only difference here is that it is convention to only return the current state, and not throw an error, in the switch default.

```js
const initialState = {
  properties: values,
};

function reducer(state = initialState, action) {
  switch (action.type) {
    // cases
    default:
      return state;
  }
}
```

### Using multiple reducers

To use multiple reducers, we need to combine them into a rootReducer using a built in redux function. This needs to be imported.

```js
import { combineReducers } from "redux";
```

When using the function, we pass in an object, and each object property represents a reducer, so give them meaningful names. We should save the output - conventionally as `rootReducer`.

```js
const rootReducer = combineReducers({
  account: accountReducer,
  customer: customerReducer,
});
```

### Create a Store

To create a store we need to import the createStore function from Redux.

```js
import { createStore } from "redux";
```

Note: this function is deprecated, but is left in the package for learning purposes.

Calling this function returns the store object which can be saved. When calling, we pass in the rootReducer function.

```js
const store = createStore(rootReducer);
```

our `store` object now has access to the following methods.

```js
store.dispatch(); // dispatch an action, like useReducer
store.getState(); // return the current state
```

### Dispatch

The dispatch method works in a similar way to dispatches with useReducer - we pass in an action. This can either be an `action object` or an `action creator function`.

```js
store.dispatch({ type: "theType", payload: "theValue" });

// or

store.dispatch(actionCreator());
```

### Actions

Since a store will typically contain multiple reducers - one for each app feature - it is convention to begin our action type names with the feature name, followed by the accion to be performed.

The payload can also be an object of multiple values.

```js
{ type: 'account/deposit', payload: amount }
{ type: 'account/withdraw', payload: amount }
{ type: 'user/updateEmail', payload: { oldEmail, newEmail } }
{ type: 'user/updateName', payload: { newName } }
```

---

# TO REVIEW

## Create a Slice

- Import the `createSlice()` function from redux toolkit.

```ts
import { createSlice } from `@reduxjs/toolkit`
```

- Use the function and store its output.
- It accepts an object as an argument, with properties `name`, `initialState`, and `reducers`.
- The `reducers` property should be an object containing reducer functions.
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

## Create the store

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

## Connecting the store to the app

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

## Typing State and Store

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

## Accessing State

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
