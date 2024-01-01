# Optimisations

## Generate Static Params

- Popular pages can be expensive to generate every time a person makes a request.
- We may want to create static versions of these pages.
- we use the `generateStaticParams( )` function
- This should return an array of the paths to be made static
- Typically used in a dynamic page
- The provided paths in the array are relative to the base path of the route
- For example `items/football-kits/lfc` might be a popular path on the `items/football-kits/[team]` dynamic route. So might `man-city`.

```ts
// page.tsx in /[team]

export async function generateStaticParams() {
  return [{ team: "lfc" }, { team: "man-city" }];
}
```

[Back to Contents](../README.md)
