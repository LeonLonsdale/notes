# State

## URL

### Write Hash

- To write to the URL, we can use an anchor tag
- The below example will add `#12313131313` to the end of the current URL.

```html
<a href="#12313131313">Click me!</a>
```

[Back to Contents](../README.md)

## Reading from the URL

### Read Hash

- Hash changes are an event on the window object: `hashchange`.
- We can therefore read the result with an event listener.
- The altering the URL is a side effect so should be done in an effect.

```ts
const [activeId, setActiveId] = useState<number | null>(null);

useEffect(() => {
  // create a handler so that we can cleanup later.
  const handleHashChange = () => {
    // retreive the hash value and slice the # off.
    // coerce the result to a number with +
    const hash = +window.location.hash.slice(1);
    // store the result in a piece of state
    setActiveId(hash);
  };
  // call the function to obtain the hash value on initial load.
  handleHashChange();

  // add event listerner for hashchange.
  window.addEventListener("hashchange", handleHashChange);

  // cleanup
  return window.removeEventListener("hashchange", handleHashChange);
}, []);
```

[Back to Contents](../README.md)
