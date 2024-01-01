# Fetch Wrapper

- A common pattern used by NextJS devs
- A component that wraps another component
- The fetch takes place in the wrapper specifically to provide the result to the child.
- The child component is therefore re-usable.

```ts
// fetch wrapper

export default async function FetchWrapper (var: string) {
    const response = await fetch(`someURL/${var}`);
    const data = await response.json();

    return (
        <ChildComponent data={data} />
    )
}
```

- The fetch code can further be abstracted into a utility function

```ts
// utils file
export const fetchData = (var: string) => {
    const response = await fetch(`someURL/${var}`);
    const data = response.json();
    return data;
}

// fetch wrapper file
import { fetchData } from '@/lib/util';

export default async function FetchWrapper (var: string) {

    const data = await fetchData(var);

    return (
        <ChildComponent data={data} />
    )
}
```

[Back to Contents](../README.md)
