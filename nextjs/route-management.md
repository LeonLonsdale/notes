# Route Management

## Create a Route

- Routes in NextJS work based on the folder structure
- The root path is `./src/app/`.
- The application homepage is `page.tsx` found in this directory.
- To create a new route, add a new directory in `.src/app/`.
- Each route must have its own `page.tsx`

```
.src/
    app/
        page.tsx
        news/
            page.tsx
        user/
            page.tsx
```

## Create a Dynamic Route

- A dynamic route is when the text in part of the path may vary
- Such as `/user/userId` where the userId will be different for each user.
- For a dynamic route we add a directory as before, but its name must be wrapped in square brackets.
- The page.tsx file must be in this directory.

```
.src/
    app/
        page.tsx
        news/
            page.tsx
        user/
            [id]
                page.tsx
```

## Get the current Pathname

- The pathname is the full path including the route, dynamic routes, queries etc.
- Accessible with a built inNextJS hook

```ts
import { usePathname } from "next/navigation";

const activePathname = usePathname();
```

## Access the Route

- We can access the route in object form using a built in nextjs hook.
- We can then manipulate this object to change the current route.

```ts
import { useRouter } from "next/navigation";

const router = useRouter();

// router now stored

router.push("route"); // go to pushed route.
```

## Access the Params

- Params are essentially what we represent with a dynamic route folder.
- We can access params using a built in NextJS Prop.
- `page.tsx` components get access to this prop by default.

```ts
type PageProps = {
    params: {
        id: string; // replace id with the dynamic route name
    }
};

export default Component({ params }: PageProps) { };
```

- In this example, `id` is the param name.
- This should match the dynamic route directory name.
- If the directory is `[city]` then replace `id` above with `city`.

## Search Params

- Search Params are also accessible by default as a prop.
- For example: `domain.com/event/city/page?=1`

```ts
type PageProps = {
    params: {
        id: string; // replace id with the dynamic route name
    },
    searchParams: {
        [key: string]: string | string[] | undefined;
    }
};

export default Component({ params, searchParams }: PageProps) { };
```

- As this could be undefiend, we need to check for it in our code.

```ts
const { page = 1 } = searchParams;

// or

const page = Number(searchParams.page) || 1; // does not allow 0. use ?? to allow 0
```
