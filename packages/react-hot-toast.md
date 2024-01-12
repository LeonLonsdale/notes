# React Hot Toast

- Used for Toast messages

```
npm i react-hot-toast
```

## Toast Position

- Choose where the toast messages should be displayed within the app.
- Import the `Toaster` component from react-hot-toaster

```ts
import { Toaster } from "react-hot-toast";
```

- Use the component in the chosen location

```ts
<Toaster />
```

- The component accepts props:
  - `position` - where should the toast appear?
    - 'bottom-right'
    - 'top-right'
    - 'bottom-left'
    - 'top-left'
    - 'top-center'
    - 'bottom-center'
  - `containerStyle`
  - `containerClassName`
  - `gutter`
  - `toastOpetions`

## Trigger Toast

- To trigger the toast message, we use the toast object.

```ts
import toast from "react-hot-toast";

toast.error(error);
```

- Other variations include:
  - toast.success()
  - toast.loading()
  - toast.custom() - accepts JSX
  - toast()
- In all cases the args are `(string, { optional options })`
- Options include:
  - `position: 'top-right'` - as above
  - `duration: number` - in milliseconds
  - `style: {}`
  - `className: ''`
  - `icon: ''`
  - `iconTheme: { primary: '#', secondary: '#' }`
  - `ariaProps: { role: '', 'aria-lie': 'polite' }`
- There is also toast.promise() which can update automatically based on promise status

```ts
const myPromise = fetchData();

toast.promise(myPromise, {
  loading: "Loading",
  success: "Got the data you asked for",
  error: "Error while fetching",
});
```

- Both success and error properties accept functions that return strings to include success/error messages.

## Toast ID

- All toast calls return a toast ID

```ts
const toastId = toast.success("All done");

toast.dismiss(toastId);
// toast.dismiss() with no ID dismisses all toasts.
```

- Toast ID can also be use to update the toast text.

```ts
const toastId = toast.loading("fetching the data");

toast.success("Got the data", { id: toastId });
```
