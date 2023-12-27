# Special Files

NextJS automatically detects and uses certain files.

## 404 Not Found Component

- Handled by default but can be customised by creation of a special component.
- Create a file named `not-found.tsx` in the `app` dir
- Create a component in this file. It's used automatically.

```ts
export default function NotFound() {
  return <main>We could not find the page you are looking for.</main>;
}
```

- In some cases, this is not automatic and our code may try to source data and return a result.
- This will cause an error.
- NextJS provides us with a `notFound()` function that we can use in such a case.
- Import this from `next/navigation`

```ts
if (!data) {
  return notFound();
}
```

## Loading states

- Create a file called `loading.tsx` in the `app` dir and export your 404 not found component.
- Next JS will use Reacts `suspense` component and load the component from this file as the fallback.
- Creating `loading.tsx` files in routes or dynamic routes mean only those routes will use the loading component.
- This allows custom loading displays/skeletons for each route.

```ts
export default function Loading() {
  return <div>Loading...</div>;
}
```

## Error Page

- The error page can be used to display errors that we did not catch.
- The errors occur on the server and are sent to the error page at the front end.
- The error page must be a `client` component.
- The component receives the `error` object and a `reset` function by default.
- The `error` object is stripped of sensitive information before being sent to the frontend.
- The `reset` function can be used in a button to attempt to retry.
- The error is of type `Error & { digest?: string}`
- The reset function is of type `() => void`.

```ts
type ErrorProps = {
  error: Error & { digest?: string };
  reset: () => void;
};

export default function Error({ error, reset }: ErrorProps) {
  return <main>// error page</main>;
}
```
