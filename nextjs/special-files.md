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
