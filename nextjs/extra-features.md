# Extra Features

## Opengraph Images

- Share links and see a preview image.
- Special file in route dir: `opengraph-image.tsx`
- Exports
  - alt text
  - image size
  - contentType
  - an image component that returns relevant text to be included in the image.
- Next creates these during build

```ts
import { ImageResponse } from "next/og";

// image metadata
export const alt = "Evento";
export const size = {
  width: 1200,
  height: 630,
};

export const contentType = "image/png";

export default async function Image({ params }: { params: { slug: string } }) {
  return new ImageResponse(
    (
      <section>
        <h1>{params.slug}</h1>
        <p>Evento - Browse events around you</p>
      </section>
    )
  );
}
```
