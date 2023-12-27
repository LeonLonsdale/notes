# Zod

For full zod documentation, visit [this page on npm](https://www.npmjs.com/package/zod)

- used for validating data

```
npm i zod
```

- import the zod object, z

```ts
import { z } from "zod";
```

## Example use with page numbers from params:

```ts
const pageNumberSchema = z.coerce.number().int().positive().optional();
```

- `pageNumberSchema` is now a schema that we can use to ensure that the page number is not a string (coerce it to a number), the number is a positive integer. It also allows the page number to be optional.
- We can pass our params into this and store the result of the test

```ts
// safeParse() prevents zod from throwing errors.
const parsedPageNumber = pageNumberSchema.safeParse(searchParams.page);
```

- `parsedPageNumber` is now an object that stores:

  - The output status, `success` as a boolean.
  - And either the `data`, if successful, or `error` if not.

  ```ts
  .safeParse(data: unknown): { success: true; data: T; } | { success: false; error: ZodError }
  ```

- We can use this in our guard clauses and to throw errrors.

```ts
if (!parsedPageNumber.success) throw new Error("Invalid page number!");
```

- If the page number was validated, we can access it on `parsedPageNumber.data`.
