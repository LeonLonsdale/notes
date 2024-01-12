# React Query

## Install

```
npm i @tanstack/react-query
```

## Query Client

- React query needs the appropriate components to be wrapped ina query provider.
- This provider is provided by react query.
- When using the provider, we must pass in the client.
- The client can be created using a contructor (`QueryClient()`) provided by react query.

```ts
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

const queryClient = new QueryClient();

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <QueryClientProvider client={queryClient}>
      <App />
    </QueryClientProvider>
  </React.StrictMode>
);
```

## useQuery()

- React query exposes the `useQuery()` hook that we can use to create our query.
- `useQuery()` accepts 3 arguments: An `array of keys`, the `fetch function`, and an `object of options`.
- The `unique key` or `array of keys`:
  - `unique key` - simply provides unique key that will be used internally for re-fetching, caching, and sharing queries.
  - `array` - the first item is a unique key, and we may need to provide more dependencies, or an object of query options.
  ```ts
  // array with key and dependency
  useQuery(['find-user]', userId], async () => fetch(`urlstring/${userId}`));
  // array with key and query options
  // get todos that are complete.
  useQuery(['todos', { isComplete: true}], () => ...)
  ```
- The `fetch function` works in the same way as we would usually use the fetch api.
- The `object of option` are optional, but may include: TODO
- The use of `useQuery()` returns an object that includes:
  - `isLoading` - loading status
  - `isError` - error status
  - `isSuccess` - success status
  - `isIdle` - query is disabled.
  - `error` - the error object
  - `data` - the result of the fetch operation
  - `isFetching` - the query is fetching, including refetching.
