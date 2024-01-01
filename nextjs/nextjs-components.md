# Built-in NextJS Components

## The Link component

- The link component creates `anchor` elements.
- Import it from `next/link`
- Accepts `href prop`
- Makes use of `children prop`.

```ts
import Link from "next/link";

<Link href="/">Home</Link>;
```

[Back to contents](#contents)

## The Image Component

- Import it from `next/image`
- Props:
  - `src=''` - the location of the image
  - `alt=''` - description of the image
  - `width={}` - width of the image
  - `height={}` - height of the image
  - `fill` - fill the current container
  - `sizes=''` - set reponsive sizes
  - `quality={}` - image quality
  - `priority` - load priority
- Width and height props are only required for images that are sourced from a different domain to reserve space for the image and avoid moving the UI around as things load.

```ts
import Image from "next/image";

<Image src="" alt="" width={} height={} />;
```

- If the image should fill its container, we can provide the `fill` keyword.
- If using `fill` we must aso provide the `sizes` property to provide responsiveness.
  For example:

```html
<image src="" alt="" fill sizes="(max-width: 1280px) 100vw, 1280px" />
```

- The Quality prop allows us to set the quality of the image.

```html
<image
  src=""
  alt=""
  fill
  sizes="(max-width: 1280px) 100vw, 1280px"
  quality="{50}"
/>
```

[Back to Contents](../README.md)
