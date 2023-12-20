# Page Setup Basics

## Set page title and description - Constant

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

## Generate metadata - Function

- Can also generate metadata to allow cusome page titles based on content.
- Do this by exporting a function named `generateMetadata`.
- This function gets access to `params` in the same way as page files.
- It's useful to Type the output of the function according to nexts `Metadata` type.
- If necessary, we can also use fetch within this function to get the data we need.

```ts
export const generateMetadata = ({ params }: Props): Metadata => {
  const { param } = params;

  return {
    title: "",
    description: "",
  };
};
```

## Favicon

To set your favicon, just put it in the `./src/app/` directory.
