# Server Actions

## 'use server' Directive

- `'use server'` can be used to mark server-side functions that can be called from client-side code.
  - Can only be used in server-side files, but can be imported to client files.
  - Can only be used on `async` functions.
- Marked functions are called `server actions`.
  - Add it at the top of the top level of an async function body, before any other code; or
  ```ts
  export const serverAction = async () => {
    "use server";
    // code
  };
  ```
  - Add it at the top of a file, before all imports, to mark all exports within the file.
  ```ts
  // top of server-actions.ts file
  "use server";
  ```
- Server actions are designed for server-side state mutations.

## Security

- Arguments passed into server actions are client controlled, so should always be validated.
- User permissions should always be validated before performing actions.

## Supported Arguments

- Supported:

  - string
  - number
  - bigint
  - boolean
  - undefined
  - null
  - Array
  - Map
  - Set
  - Typed Array & Array Buffer
  - Date
  - Form Data
  - Other server actions
  - Promises
  - Objects created with initializers (not classes)
  - Registered symbols

- Not Supported
  - React elements / JSX
  - Functions (other than server actions)
  - Classes or objects that are instances of classes
  - Unregistered symbols

## Server Actions in Forms

- Passed into the `action` attribute on a form

```html
<form action="{serverAction}"></form>
```

- `FormData` is passed to the server action by React automatically.

```ts
export const serverAction = async (formData: FormData) => {};
```
