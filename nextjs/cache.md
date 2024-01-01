# Caching

## unstable_cache

- Should be stable soon.
- When using an ORM for database requests we lose the caching built into fetch API by next.
- To return similar functionality we can wrap our database requests in `unstable_cache()`.
- Import it from `next/cache`

```ts
import { unstable_cache } from "next/cache";
```

- it accepts a callback, which is what will retrieve the data from the database.

```ts
export const results = unstable_cache(async () => {
  // get stuff
  // return stuff
});
```

[Back to Contents](../README.md)
