# Data Fetching Strategies

## Choosing Between SSG, SSR, CSR
### Best Practices for Data Fetching

**Example:**

1. **Static Site Generation (SSG) with `getStaticProps`:**
   ```tsx
   // pages/index.tsx
   import { GetStaticProps } from 'next';
   
   const Home: React.FC<{ data: string[] }> = ({ data }) => (
     <div>
       <h1>Static Site Generation</h1>
       <ul>
         {data.map((item) => (
           <li key={item}>{item}</li>
         ))}
       </ul>
     </div>
   );
   
   export const getStaticProps: GetStaticProps = async () => {
     const data = await fetch('https://api.example.com/items').then((res) => res.json());
     return { props: { data } };
   };
   
   export default Home;
   ```

2. **Server-Side Rendering (SSR) with `getServerSideProps`:**
   ```tsx
   // pages/ssr.tsx
   import { GetServerSideProps } from 'next';
   
   const SSRPage: React.FC<{ data: string[] }> = ({ data }) => (
     <div>
       <h1>Server-Side Rendering</h1>
       <ul>
         {data.map((item) => (
           <li key={item}>{item}</li>
         ))}
       </ul>
     </div>
   );
   
   export const getServerSideProps: GetServerSideProps = async () => {
     const data = await fetch('https://api.example.com/items').then((res) => res.json());
     return { props: { data } };
   };
   
   export default SSRPage;
   ```

3. **Client-Side Rendering (CSR) with `useEffect`:**
   ```tsx
   // pages/csr.tsx
   import React, { useEffect, useState } from 'react';
   
   const CSRPage: React.FC = () => {
     const [data, setData] = useState<string[]>([]);
     useEffect(() => {
       fetch('https://api.example.com/items')
         .then((res) => res.json())
         .then((data) => setData(data));
     }, []);
   
     return (
       <div>
         <h1>Client-Side Rendering</h1>
         <ul>
           {data.map((item) => (
             <li key={item}>{item}</li>
           ))}
         </ul>
       </div>
     );
   };
   
   export default CSRPage;
   ```

**Comments:**
- **SSG**: Ideal for static content, built at build time.
- **SSR**: Suitable for dynamic content, fetched at request time.
- **CSR**: Used for client-side data fetching post-render.



## Caching Data with ISR
### Setting Revalidation Times

**Example:**

```tsx
// pages/index.tsx
import { GetStaticProps } from 'next';

// Component to display data
const Home: React.FC<{ data: string[] }> = ({ data }) => (
  <div>
    <h1>Incremental Static Regeneration</h1>
    <ul>
      {data.map((item) => (
        <li key={item}>{item}</li>
      ))}
    </ul>
  </div>
);

// ISR configuration with revalidation time
export const getStaticProps: GetStaticProps = async () => {
  // Fetch data
  const data = await fetch('https://api.example.com/items').then((res) => res.json());

  // Return props with revalidate time
  return {
    props: { data },
    revalidate: 10, // Regenerate page every 10 seconds
  };
};

export default Home;
```

**Comments:**
- **ISR (Incremental Static Regeneration)**: Allows static pages to be updated after deployment.
- **Revalidation Time**: Page is regenerated at most every `n` seconds, ensuring fresh data.



## Handling API Failures
### Graceful Degradation

**Example:**

```tsx
// pages/index.tsx
import { GetStaticProps } from 'next';

// Component to display data
const Home: React.FC<{ data: string[]; error?: string }> = ({ data, error }) => (
  <div>
    <h1>Data Fetching with Error Handling</h1>
    {error ? (
      <p>Error loading data: {error}</p>
    ) : (
      <ul>
        {data.map((item) => (
          <li key={item}>{item}</li>
        ))}
      </ul>
    )}
  </div>
);

// Fetch data with error handling
export const getStaticProps: GetStaticProps = async () => {
  try {
    const res = await fetch('https://api.example.com/items');
    
    // Check if response is ok
    if (!res.ok) throw new Error('Failed to fetch data');
    
    const data = await res.json();
    
    // Return data
    return {
      props: { data },
      revalidate: 10, // Regenerate page every 10 seconds
    };
  } catch (error) {
    // Return error message if fetching fails
    return {
      props: { data: [], error: (error as Error).message },
    };
  }
};

export default Home;
```

**Comments:**
- **Graceful Degradation**: Handles API failures by providing an error message or fallback content.
- **Error Handling**: Uses try-catch to manage fetch errors and ensures user experience is maintained even when data fetch fails.