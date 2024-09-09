# Middleware

## Using Middleware in Next.js
### Auth Checks, Logging, Caching

Middleware in Next.js handles requests before reaching routes. Use it for authentication, logging, or caching. Place middleware in `middleware.ts`.

**Example:**

```ts
// middleware.ts
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

// Auth Check Middleware
export function middleware(req: NextRequest) {
  const token = req.cookies.get('auth_token');
  if (!token) {
    return NextResponse.redirect('/login');
  }
  // Logging Request
  console.log(`Request URL: ${req.url}`);
  return NextResponse.next();
}

// Apply middleware to specific paths
export const config = {
  matcher: ['/protected/*'],
};
```

In this example, unauthorized requests are redirected to `/login`, and each request URL is logged.