# Page Setup Basics

## Set page title and description

- Access `./src/layout.tsx`
- Look for the `metadata` constant
- Edit object values as required

```ts
export const metadata: Metadata = {
  title: "Page title",
  description: "Page description",
};
```

- Different pages can have their own metadata constant export if required.

[Back to contents](#contents)

## Favicon

To set your favicon, just put it in the `./src/app/` directory.
