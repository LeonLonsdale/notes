# Middleware

## Page redirect

- One common use of middleware is page redirects.
- Particularly useful for dynamic routes, where the base route should not display a page.
- For example where `items/all` should be used, but `items/` is enterered we may want to redirect.

```ts
import { NextRequest, NextResponse } from "next/server";

export function middleware(request: NextRequest) {
  // does not accept relative paths so we must create an absolute path
  // new URL() accepts the relative path and the base path and combines them.
  return NextResponse.redirect(new URL("/items/all", request.url));
}

export const config = {
  matcher: ["/events"],
};
```

[Back to Contents](../README.md)
