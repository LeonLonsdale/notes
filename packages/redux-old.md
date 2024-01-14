# Oldschool Redux

## Initial State & Reducers

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

## Using multiple reducers

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

## Create a Store

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

## Dispatch

The dispatch method works in a similar way to dispatches with useReducer - we pass in an action. This can either be an `action object` or an `action creator function`.

```js
store.dispatch({ type: "theType", payload: "theValue" });

// or

store.dispatch(actionCreator());
```

## Actions

Since a store will typically contain multiple reducers - one for each app feature - it is convention to begin our action type names with the feature name, followed by the accion to be performed.

The payload can also be an object of multiple values.

```js
{ type: 'account/deposit', payload: amount }
{ type: 'account/withdraw', payload: amount }
{ type: 'user/updateEmail', payload: { oldEmail, newEmail } }
{ type: 'user/updateName', payload: { newName } }
```
