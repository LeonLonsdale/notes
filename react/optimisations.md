# Optimisations

## Debouncing

- Use debouncing whenever we are listening to events that happen frequently and the resulting actions may, therefore, cause the app to lag.
- Debouncing allows us to add a delay to the resulting action.
- Easily achieved using a custom hook.

```ts
const useDebounce = <T>(value: T, delay = 500): T => {
  const [debounce, setDebounce] = useState<T>(value);

  useEffect(() => {
    const id = setTimeout(() => setDebounce(value), delay);

    return clearTimeout(id);
  }, [value, delay]);

  return debounce;
};
```

[Back to Contents](../README.md)
