# Rendering Modes

## Static Site Generation (SSG)
### 1. Benefits and Use Cases

**Benefits & Use Cases**: SSG generates static HTML pages at build time, offering fast page loads and better SEO. Ideal for content that doesn’t change often.

**Example:**

1. **Page with SSG:**
   - File: `pages/ssg-example.tsx`
   ```tsx
   // pages/ssg-example.tsx
   import { GetStaticProps } from 'next';
   
   interface Props {
     data: string;
   }
   
   const SSGPage = ({ data }: Props) => {
     return (
       <div>
         <h1>Static Site Generation Example</h1>
         <p>{data}</p>
       </div>
     );
   };
   
   export const getStaticProps: GetStaticProps = async () => {
     // Simulating data fetching
     const data = 'This data is generated at build time';
   
     return {
       props: { data },
     };
   };
   
   export default SSGPage;
   ```

**Comments:**
- **`getStaticProps`**: Fetches data at build time.
- **Static HTML**: Pages are pre-rendered into static HTML for fast loading.
- **Best Use**: Suitable for blogs, documentation, or other static content.

### 2. Incremental Static Regeneration (ISR)

**ISR** allows updating static pages after deployment without rebuilding the entire site. Ideal for content that updates frequently.

**Example:**

1. **Page with ISR:**
   
   - File: `pages/ISR-example.tsx`
   ```tsx
   // pages/ISR-example.tsx
   import { GetStaticProps } from 'next';
   
   interface Props {
     data: string;
   }
   
   const ISRPage = ({ data }: Props) => {
     return (
       <div>
         <h1>Incremental Static Regeneration Example</h1>
         <p>{data}</p>
       </div>
     );
   };
   
   export const getStaticProps: GetStaticProps = async () => {
     // Simulating data fetching
     const data = 'This data is regenerated incrementally';
   
     return {
       props: { data },
       revalidate: 60, // Regenerate page every 60 seconds
     };
   };
   
   export default ISRPage;
   ```

**Comments:**
- **`revalidate`**: Time in seconds to wait before regenerating the page.
- **ISR**: Updates static content at runtime, providing fresh data without a full rebuild.



## Server-side Rendering (SSR)

### Use Cases and Performance

**SSR** generates pages on each request, ideal for dynamic content and personalization.

**Example:**

1. **Page with SSR:**
   - File: `pages/SSR-example.tsx`
   ```tsx
   // pages/SSR-example.tsx
   import { GetServerSideProps } from 'next';
   
   interface Props {
     data: string;
   }
   
   const SSRPage = ({ data }: Props) => {
     return (
       <div>
         <h1>Server-Side Rendering Example</h1>
         <p>{data}</p>
       </div>
     );
   };
   
   export const getServerSideProps: GetServerSideProps = async () => {
     // Simulating data fetching from server
     const data = 'This data is fetched on each request';
   
     return {
       props: { data },
     };
   };
   
   export default SSRPage;
   ```

**Comments:**
- **`getServerSideProps`**: Fetches data on each request.
- **SSR**: Suitable for dynamic content and ensures fresh data but can be slower than static generation.



## Client-side Rendering (CSR)
### Differences from SSG and SSR
**CSR** renders content on the client, ideal for user interactions and dynamic updates.

**Example:**

1. **Page with CSR:**
   - File: `pages/CSR-example.tsx`
   ```tsx
   // pages/CSR-example.tsx
   import { useEffect, useState } from 'react';
   
   const CSRPage = () => {
     const [data, setData] = useState<string | null>(null);
   
     useEffect(() => {
       // Simulating client-side data fetching
       fetch('/api/data')
         .then((response) => response.json())
         .then((data) => setData(data.message));
     }, []);
   
     return (
       <div>
         <h1>Client-Side Rendering Example</h1>
         <p>{data || 'Loading...'}</p>
       </div>
     );
   };
   
   export default CSRPage;
   ```

**Comments:**
- **CSR**: Renders content on the client, enabling dynamic data fetching and interactivity.
- **Differences**: 
  - **SSG**: Static generation at build time.
  - **SSR**: Server-side rendering on each request.
  - **CSR**: Client-side rendering after page load.



## Hybrid Rendering

### Combining SSG, SSR, and CSR

**Hybrid Rendering** allows combining SSG, SSR, and CSR in a single application for optimized performance and flexibility.

**Example:**

1. **Page with Hybrid Rendering:**
   - File: `pages/hybrid-example.tsx`
   ```tsx
   // pages/hybrid-example.tsx
   import { GetStaticProps, GetServerSideProps } from 'next';
   import { useEffect, useState } from 'react';
   
   type Props = {
     staticData: string;
     serverData: string;
   };
   
   const HybridPage = ({ staticData, serverData }: Props) => {
     const [clientData, setClientData] = useState<string | null>(null);
   
     useEffect(() => {
       // Client-side data fetching
       fetch('/api/client-data')
         .then((response) => response.json())
         .then((data) => setClientData(data.message));
     }, []);
   
     return (
       <div>
         <h1>Hybrid Rendering Example</h1>
         <p>Static Data: {staticData}</p>
         <p>Server-Side Data: {serverData}</p>
         <p>Client-Side Data: {clientData || 'Loading...'}</p>
       </div>
     );
   };
   
   export const getStaticProps: GetStaticProps = async () => {
     // Static generation
     const staticData = 'Static data fetched at build time';
     return { props: { staticData } };
   };
   
   export const getServerSideProps: GetServerSideProps = async () => {
     // Server-side rendering
     const serverData = 'Server-side data fetched on request';
     return { props: { serverData } };
   };
   
   export default HybridPage;
   ```

**Comments:**
- **Hybrid Rendering** combines SSG (static data), SSR (server-side data), and CSR (client-side data).
- **SSG**: Pre-fetches data at build time.
- **SSR**: Fetches data on each request.
- **CSR**: Fetches data after page load.
