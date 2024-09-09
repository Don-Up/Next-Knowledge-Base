# Routing

## Defining Simple Routes
### Linking Pages with Link Component

**Example:**

1. **Home Page:**
   - File: `pages/index.tsx`
   ```tsx
   // pages/index.tsx
   import Link from 'next/link';
   import React from 'react';
   
   const Home: React.FC = () => (
     <div>
       <h1>Home Page</h1>
       <Link href="/about">
         <a>Go to About Page</a>
       </Link>
     </div>
   );
   
   export default Home;
   ```

2. **About Page:**
   - File: `pages/about.tsx`
   ```tsx
   // pages/about.tsx
   import React from 'react';
   
   const About: React.FC = () => (
     <div>
       <h1>About Page</h1>
     </div>
   );
   
   export default About;
   ```

**Comments:**
- **Link Component**: Use `Link` from `next/link` to navigate between pages.
- **Home Page**: Contains a link to the About page.
- **About Page**: Simple page content for demonstration.



## Nested and Dynamic Routes
### 1. Handling Dynamic Segments

**Example:**

1. **Nested Route Setup:**
   - File: `pages/products/[id].tsx`
   ```tsx
   // pages/products/[id].tsx
   import { GetServerSideProps } from 'next';
   import React from 'react';
   
   interface Props {
     product: { id: string; name: string };
   }
   
   const Product: React.FC<Props> = ({ product }) => (
     <div>
       <h1>Product: {product.name}</h1>
     </div>
   );
   
   export const getServerSideProps: GetServerSideProps = async (context) => {
     const { id } = context.params as { id: string };
     // Mock data fetching
     const product = { id, name: `Product ${id}` };
     return { props: { product } };
   };
   
   export default Product;
   ```

2. **Link to Dynamic Route:**
   - File: `pages/index.tsx`
   ```tsx
   // pages/index.tsx
   import Link from 'next/link';
   import React from 'react';
   
   const Home: React.FC = () => (
     <div>
       <h1>Home Page</h1>
       <Link href="/products/1">
         <a>Go to Product 1</a>
       </Link>
     </div>
   );
   
   export default Home;
   ```

**Comments:**
- **Dynamic Route**: `[id].tsx` handles dynamic segments in URL.
- **Data Fetching**: Use `getServerSideProps` to fetch data based on dynamic segments.
- **Link Component**: Navigate to dynamic routes using `Link`.

### 2. Optional Catch-All Routes ([[...slug]].js)

**Example:**

1. **Optional Catch-All Route Setup:**
   - File: `pages/[...slug].tsx`
   ```tsx
   // pages/[...slug].tsx
   import { GetServerSideProps } from 'next';
   import React from 'react';
   
   interface Props {
     slug: string[];
   }
   
   const CatchAll: React.FC<Props> = ({ slug }) => (
     <div>
       <h1>Catch-All Route</h1>
       <p>Slug: {slug.join('/')}</p>
     </div>
   );
   
   export const getServerSideProps: GetServerSideProps = async (context) => {
     const slug = context.params?.slug as string[] || [];
     return { props: { slug } };
   };
   
   export default CatchAll;
   ```

2. **Link to Optional Catch-All Route:**
   - File: `pages/index.tsx`
   ```tsx
   // pages/index.tsx
   import Link from 'next/link';
   import React from 'react';
   
   const Home: React.FC = () => (
     <div>
       <h1>Home Page</h1>
       <Link href="/hello/world">
         <a>Go to /hello/world</a>
       </Link>
     </div>
   );
   
   export default Home;
   ```

**Comments:**
- **Optional Catch-All Route**: `[[...slug]].tsx` captures multiple URL segments.
- **Data Fetching**: Use `getServerSideProps` to handle dynamic segments.
- **Link Component**: Navigate using `Link` to catch-all routes.



## Custom 404 Pages
### Creating Custom Error Pages

**Example:**

1. **Custom 404 Page Setup:**
   - File: `pages/404.tsx`
   ```tsx
   // pages/404.tsx
   import React from 'react';
   
   const Custom404: React.FC = () => (
     <div style={{ textAlign: 'center', padding: '50px' }}>
       <h1>404 - Page Not Found</h1>
       <p>The page you are looking for does not exist.</p>
     </div>
   );
   
   export default Custom404;
   ```

2. **Handling Other Errors:**
   - File: `pages/_error.tsx`
   ```tsx
   // pages/_error.tsx
   import React from 'react';
   import { NextPageContext } from 'next';
   
   interface Props {
     statusCode: number;
   }
   
   const Error: React.FC<Props> = ({ statusCode }) => (
     <div style={{ textAlign: 'center', padding: '50px' }}>
       <h1>{statusCode} - Error</h1>
       <p>Something went wrong. Please try again later.</p>
     </div>
   );
   
   Error.getInitialProps = async (ctx: NextPageContext) => {
     const statusCode = ctx.res?.statusCode || ctx.err?.statusCode || 500;
     return { statusCode };
   };
   
   export default Error;
   ```

**Comments:**
- **Custom 404 Page**: Displayed when a route is not found.
- **Error Handling**: Use `_error.tsx` for other server or client errors.
- **Styling**: Simple center-aligned text for user-friendly error messages.