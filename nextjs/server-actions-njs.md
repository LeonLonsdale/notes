# Server Actions in NextJS

- See [server actions](./../react/server-actions.md) for basic react functionality.
- To use server actions in NextJS we need to opt-in.
- Do this in the next.config.js file by adding the following property:

```ts
experimental: {
    serverActions: true,
}
```

- We have some useful hooks that we can use alongside server actions for:
  - UI feedback
    - Re-validation - `revalidatePath()` - imports from `next/cache`.
    - Reset forms
    - Visualise status - `useFormStatus()` - imports from `react-dom`
  - Optimistic rendering - `useOptimistic()` - imports from `react`
  - Error handling

## UI Feedback

- Since the server actions are async, they may take some time to complete.
- To provide a better user experience, we will want to display some feedback:
  - A loading message or icon.
  - Disable a button and / or inputs
- You may also want to reset a form.
- Any client interactivity needs to be done in a `client component` file. Refactoring into new client components may therefore be necessary, and the server action can be imported.

### Re-validation

- When the server action is complete, we don't want to user to have to refresh the page to see the changes.
- We can make the component revalidate using a nextJS hook, `revalidatePath()`.
- We pass the path that we want to revalidate in as an argument.

```ts
revalidatePath("/");
```

- This will then revalidate the data and drop the cache if anything has changed, and re-render any applicable components.

### Form Resets

- We now pass a function into the `action` attribute that calls the server action.
- This only works in NextJS

```html
<form action={async (formData) => serverAction(formData)}></form>
```

- Within this function we can now also reset the form by resetting state (controlled input) or with useRef in an uncontrolled input.

```ts
const ref = useRef<HTMLFormElement>(null);

export default function Form() {
    return (
      // Controlled Input
      <form
        action={async (formData: string) => {
          serverAction(formData);
          setState('');
        }}
      >
        <input value={state} />
      </form>

      // Uncontrolled Input
      <form
        ref={ref}
        action={async (formData) => {
          ref.current?.reset();
          serverAction(formData);
        }}
      >
        <input />
      </form>
    )
}
```

### Status

- We also have access to a hook that allows us to see the current status of the server action - the pending state.
- This hook is `useFormStatus` (experimental at the time of writing)
- It must be used in a client component where the component is a child of the component in which the server action was called.
- The hook returns an object containing:
  - The action (the server action)
  - Data (the form data)
  - Method
  - Pending state
- We can deconstruct that pending state.

```ts
<app>
  <ServerComponent>
    <FormComponent>
      {" "}
      // server action called here
      <Child></Child> // use the hook here
    </FormComponent>
  </ServerComponent>
</app>
```

```ts
export default function Button() {
  const { pending } = useFormStatus();
  return (
    <button disabled={pending}>
      {pending ? "In progress..." : "Click Me"}
    </button>
  );
}
```

## Optimistic Rendering

- For a better user experience, we may wish to render the update right away so the user is not waiting.
- We can remove it again if the action fails.
- We can do this with the `useOptimistic` hook (also experimental).
- This accepts the current state and an updater function.
- The updater function accepts the current state and the new value as args.
- This returns the `optimistic state` and a dispatching function.

```ts
const [optimisticState, addOptimistic] = useOptimistic(state, updateFn(state, newValue) => {})
```

- For example:

```js
const todos = ['cut grass', 'buy milk'];
const newTodo = 'learn NextJS';

// somewhere in a server action, far, far away
const [optimisticTodos, addOptimisticTodo] = useOptimistic(todos,(state, newTodo) => {
    return [...state, newTodo];
}

// call addOptimisticTodo
addOptimisticTodo(newTodo);
```

- `optimisticTodos` will include `newTodo` even if the server request has not yet finished.
- We can use this for immediate rendering.

## Error Handling

- As with any server request, the request may fail.
- We need some way of catching the error and sending feedback to the client.
- The simplest way to do this is to wrap the body of the server action in a `try`/`catch` block, and return the error to the client.
