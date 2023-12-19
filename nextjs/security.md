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
