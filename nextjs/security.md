# Security

## Cross-origin sites

- By default, NextJS blocks resources that are not locally accessible.
- To allow access to the resources we need, we have to edit the `next.config.js`.

### For Images

- Create an `image` property in the config file. This should be an object.
- Add an object property `remotePatterns` which should be an array.
- Add an object to the array with the properties `protocol` and `hostname`.
- Set the protocol value to `https` and the hostname value to the domain the image is sourced from.

```js
const nextConfig = {
  images: {
    remotePatterns: [{ protocol: "https", hostname: "bytegrad.com" }],
  },
};
```

[Back to Contents](../README.md)

## Server-only utilities

- Some functions may reveal sensitive information if used on in a client component.
- For example: database requests may include API keys.
- We don't want these utility functions to be called in client components, but by default there's nothing stopping us from doing so.
- We should first put these util functions in a new file. Such as `server-utils.ts`.
- Install the `server-only` package

```
npm i server-only
```

- Import the package at the root of the document

```ts
import "server-only";
```

- This will now throw errors if attempting to use any function from this file in a client component.

[Back to Contents](../README.md)
