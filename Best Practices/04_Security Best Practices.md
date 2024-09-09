# Security Best Practices

## Protecting Against XSS, CSRF
### 1. Using HTTP Headers

Enhance security by using HTTP headers to prevent XSS (Cross-Site Scripting) and CSRF (Cross-Site Request Forgery) attacks.

**Example Setup:**

1. **Add Security Headers with `next.config.js`:**

   ```ts
   // next.config.js
   module.exports = {
     async headers() {
       return [
         {
           // Apply these headers to all routes
           source: '/(.*)',
           headers: [
             {
               key: 'Content-Security-Policy',
               value: "default-src 'self'; script-src 'self'; style-src 'self';",
             },
             {
               key: 'X-Content-Type-Options',
               value: 'nosniff',
             },
             {
               key: 'X-Frame-Options',
               value: 'DENY',
             },
             {
               key: 'X-XSS-Protection',
               value: '1; mode=block',
             },
             {
               key: 'Strict-Transport-Security',
               value: 'max-age=31536000; includeSubDomains',
             },
             {
               key: 'Referrer-Policy',
               value: 'no-referrer',
             },
           ],
         },
       ];
     },
   };
   ```

2. **Configure CSRF Protection (API Routes Example):**

   ```ts
   // pages/api/your-endpoint.ts
   import { NextApiRequest, NextApiResponse } from 'next';
   import { getCsrfToken } from 'next-auth/react';
   import { NextApiRequestWithCsrf } from '../../lib/csrf';
   
   export default async function handler(req: NextApiRequestWithCsrf, res: NextApiResponse) {
     // Check CSRF token
     const csrfToken = await getCsrfToken({ req });
     if (req.method === 'POST') {
       if (req.headers['x-csrf-token'] !== csrfToken) {
         return res.status(403).json({ error: 'Invalid CSRF token' });
       }
       // Handle POST request
     }
     res.status(200).json({ message: 'Success' });
   }
   ```

**Code Example:**

```ts
// lib/csrf.ts
import { NextApiRequest } from 'next';

export interface NextApiRequestWithCsrf extends NextApiRequest {
  csrfToken?: string;
}
```

**Deploy and Verify:**
- Ensure headers are set in production.
- Test endpoints for CSRF protection.

Using security headers and CSRF tokens helps protect your Next.js application from XSS and CSRF attacks.

### 2. Securing API Routes

Protect API routes from XSS (Cross-Site Scripting) and CSRF (Cross-Site Request Forgery) attacks by sanitizing inputs and validating CSRF tokens.

**Example Setup:**

1. **Sanitize Inputs:**

   ```ts
   // utils/sanitize.ts
   import xss from 'xss';

   export const sanitizeInput = (input: string) => xss(input);
   ```

2. **CSRF Protection in API Routes:**

   ```ts
   // pages/api/secure-endpoint.ts
   import { NextApiRequest, NextApiResponse } from 'next';
   import { getCsrfToken } from 'next-auth/react';
   import { sanitizeInput } from '../../utils/sanitize';
   
   export default async function handler(req: NextApiRequest, res: NextApiResponse) {
     // Check CSRF token for POST requests
     const csrfToken = await getCsrfToken({ req });
     if (req.method === 'POST') {
       if (req.headers['x-csrf-token'] !== csrfToken) {
         return res.status(403).json({ error: 'Invalid CSRF token' });
       }
       // Sanitize user input
       const sanitizedData = sanitizeInput(req.body.data);
       // Handle sanitized data
       res.status(200).json({ message: 'Data received and sanitized', data: sanitizedData });
     } else {
       res.status(405).json({ error: 'Method not allowed' });
     }
   }
   ```

**Code Example:**

```ts
// pages/api/secure-endpoint.ts
import { NextApiRequest, NextApiResponse } from 'next';

export default async function handler(req: NextApiRequest, res: NextApiResponse) {
  if (req.method === 'POST') {
    // Example of sanitizing input
    const sanitizedInput = sanitizeInput(req.body.input);
    // Example of setting a CSRF token header
    const csrfToken = await getCsrfToken({ req });
    
    if (req.headers['x-csrf-token'] !== csrfToken) {
      return res.status(403).json({ error: 'Invalid CSRF token' });
    }
    res.status(200).json({ message: 'Success', data: sanitizedInput });
  } else {
    res.status(405).json({ error: 'Method not allowed' });
  }
}
```

**Deploy and Verify:**
- Ensure input is sanitized before use.
- Verify CSRF tokens are validated on API requests.

Using input sanitization and CSRF tokens enhances API route security in Next.js applications.