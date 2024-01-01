# NextJS Functions

- Next JS exposes access to some useful functions that we can use.

## revalidatePath()

- Allows us to purge cached data.
- By default, the revalidation happens on the next page visit.
- In server actions, they will invalidate the cache and re-render relevant components in the same request.

```ts
revalidatePath(path: string, type?: 'page' | 'layout'): void;
```

[Back to Contents](../README.md)
