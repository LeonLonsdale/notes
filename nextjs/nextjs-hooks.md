# NextJS Hooks & Props

## Get the current path

```ts
// get the current pathname ( route + params )
import { usePathname } from "next/navigation";

const path = usePathname();
```

## Access the current route

```ts
// access the current route
import { useRouter } from "next/navigation";

const router = useRouter();

router.push("newRoute");
```

## Access the Params

```ts
// access the params
// prop available to all page.tsx components.
// key === name of dynamic route

type PageProps = {
  params: {
    key: string;
  };
};

export default function Component({ params }: PageProps) {
  const { key } = params;
}
```
