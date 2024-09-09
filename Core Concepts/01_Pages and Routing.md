# Pages and Routing

## File-based Routing
### 1. Creating Pages with Files

In Next.js, pages are created by adding `.js` or `.tsx` files in the `pages` directory. Each file automatically becomes a route based on its file name.

**Example:**

1. **Creating a Page:**

   - File: `pages/about.tsx`

   ```tsx
   // pages/about.tsx
   import React from 'react';
   
   // About page component
   const About: React.FC = () => {
     return (
       <div>
         <h1>About Us</h1>
         <p>Welcome to the About page!</p>
       </div>
     );
   };
   
   export default About;
   ```

2. **Automatic Routing:**

   - Access by navigating to `/about` in your browser.

**Comments:**

- Each file in `pages` maps to a route.
- File name defines the URL path.
- No explicit routing configuration needed.

### 2. Dynamic Routes ([id].js)

Next.js supports dynamic routing by using square brackets in the file name. This allows creating pages that match dynamic URL segments.

**Example:**

1. **Creating a Dynamic Route:**
   
   - File: `pages/products/[id].tsx`
   ```tsx
   // pages/products/[id].tsx
   import { useRouter } from 'next/router';
   import React from 'react';
   
   // Dynamic product page component
   const Product: React.FC = () => {
     const router = useRouter();
     const { id } = router.query; // Extract the dynamic 'id' from the URL
   
     return (
       <div>
         <h1>Product Details</h1>
         <p>Product ID: {id}</p>
       </div>
     );
   };
   
   export default Product;
   ```
   
2. **Accessing Dynamic Routes:**
   - URL `/products/123` will render the component, displaying "Product ID: 123".

**Comments:**
- Files named with `[id].tsx` represent dynamic routes.
- `useRouter` hook is used to access dynamic parameters.
- Supports multiple dynamic segments like `[category]/[id].tsx`.



## Nested Routes

### Folder Structure for Nested Routes

Next.js uses a folder-based routing system, allowing you to create nested routes by organizing files within subfolders.

**Example:**

1. **Folder Structure for Nested Routes:**
   ```
   /pages
   └── /blog
       ├── index.tsx        // Renders /blog
       └── /[slug]
           ├── index.tsx    // Renders /blog/[slug]
           └── /comments
               └── index.tsx // Renders /blog/[slug]/comments
   ```

2. **Sample Component for Nested Route:**
   
   - File: `pages/blog/[slug]/comments/index.tsx`
   ```tsx
   // pages/blog/[slug]/comments/index.tsx
   import { useRouter } from 'next/router';
   import React from 'react';
   
   // Nested comments page component
   const Comments: React.FC = () => {
     const router = useRouter();
     const { slug } = router.query; // Extract 'slug' parameter from the URL
   
     return (
       <div>
         <h2>Comments for Blog Post: {slug}</h2>
         {/* Add comments list or form here */}
       </div>
     );
   };
   
   export default Comments;
   ```

**Comments:**
- Organize files into subfolders to create nested routes.
- Each `index.tsx` in a folder renders the respective path.
- `[slug]` is a dynamic route parameter accessed using `useRouter`.
- Nested routes enhance structuring of complex paths like `/blog/[slug]/comments`.



## API Routes

### 1. Creating API Endpoints

Next.js allows you to create serverless API endpoints by placing files inside the `pages/api` directory. Each file maps to an API endpoint.

**Example:**

1. **Creating an API Endpoint:**
   
   - File: `pages/api/hello.ts`
   ```tsx
   // pages/api/hello.ts
   import type { NextApiRequest, NextApiResponse } from 'next';
   
   // API handler function
   export default function handler(req: NextApiRequest, res: NextApiResponse) {
     // Handle GET requests
     if (req.method === 'GET') {
       res.status(200).json({ message: 'Hello, Next.js API!' });
     } else {
       // Handle other request methods
       res.setHeader('Allow', ['GET']);
       res.status(405).end(`Method ${req.method} Not Allowed`);
     }
   }
   ```

**Comments:**
- **Endpoint URL**: The above file creates an API endpoint at `/api/hello`.
- **Request & Response**: Uses `NextApiRequest` and `NextApiResponse` for request and response objects.
- **HTTP Methods**: Handles GET requests; restricts others with a 405 status (Method Not Allowed).
- **Response**: Sends a JSON response with a message when accessed via GET.

This structure allows you to build APIs easily within your Next.js application, leveraging the power of serverless functions.

### 2. Server-side Logic with API Routes

Next.js API routes can handle server-side logic such as data fetching, processing, and database interactions. This allows you to run server-side code in response to HTTP requests.

**Example:**

1. **API Endpoint with Server-side Logic:**
   - File: `pages/api/data.ts`
   ```tsx
   // pages/api/data.ts
   import type { NextApiRequest, NextApiResponse } from 'next';
   
   // Simulate server-side logic, like fetching data from a database
   async function fetchData() {
     // This could be a database call
     return new Promise((resolve) => {
       setTimeout(() => {
         resolve({ data: ['Item 1', 'Item 2', 'Item 3'] });
       }, 1000); // Simulating delay
     });
   }
   
   // API handler function
   export default async function handler(req: NextApiRequest, res: NextApiResponse) {
     try {
       // Perform server-side logic
       const data = await fetchData();
       
       // Send the data as a JSON response
       res.status(200).json(data);
     } catch (error) {
       // Handle errors gracefully
       res.status(500).json({ error: 'Failed to fetch data' });
     }
   }
   ```

**Comments:**
- **Server-side Logic**: `fetchData` simulates a server-side operation like fetching data from a database or external API.
- **Handling Async Operations**: Uses `async/await` to handle asynchronous logic inside API routes.
- **Error Handling**: Catches errors and responds with a 500 status code if something goes wrong.
- **Response**: Returns fetched data as JSON with a 200 status code on success.

This setup allows Next.js API routes to execute complex server-side operations, making them powerful tools for backend logic within a Next.js application.