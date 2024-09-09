# Error Handling

## Custom Error Pages
### Creating _error.js

**Example:**

```tsx
// pages/_error.tsx
import { NextPageContext } from 'next';
import { FC } from 'react';

// Custom Error Page Component
const ErrorPage: FC<{ statusCode?: number }> = ({ statusCode }) => (
  <div>
    <h1>{statusCode ? `Error ${statusCode}` : 'An error occurred'}</h1>
    <p>Sorry, something went wrong.</p>
  </div>
);

// Get Initial Props for Error Page
ErrorPage.getInitialProps = async (ctx: NextPageContext) => {
  const statusCode = ctx.res?.statusCode || ctx.err?.statusCode || 404;
  return { statusCode };
};

export default ErrorPage;
```

**Comments:**
- **`_error.tsx`**: Custom error page component for handling errors.
- **`getInitialProps`**: Determines the error status code for rendering.



## Handling Errors in getServerSideProps
### Using Try-Catch Blocks

**Example:**

```tsx
// pages/example.tsx
import { GetServerSideProps, NextPage } from 'next';

interface Props {
  data?: string;
  error?: string;
}

// Page Component
const ExamplePage: NextPage<Props> = ({ data, error }) => (
  <div>
    {error ? <p>Error: {error}</p> : <p>Data: {data}</p>}
  </div>
);

// Server-side Data Fetching with Error Handling
export const getServerSideProps: GetServerSideProps = async () => {
  try {
    // Simulate data fetching
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) throw new Error('Failed to fetch data');
    const data = await response.json();
    return { props: { data: data.message } };
  } catch (error) {
    return { props: { error: error.message } };
  }
};

export default ExamplePage;
```

**Comments:**
- **`getServerSideProps`**: Fetches data and handles errors with a try-catch block.
- **Error Handling**: Displays an error message if data fetching fails.



## Graceful Degradation for Data Fetching
### Fallbacks in SSG and ISR

**Example:**

```tsx
// pages/example.tsx
import { GetStaticProps, NextPage } from 'next';

interface Props {
  data?: string;
  error?: string;
}

// Page Component
const ExamplePage: NextPage<Props> = ({ data, error }) => (
  <div>
    {error ? <p>Error: {error}</p> : <p>Data: {data}</p>}
  </div>
);

// Static Generation with Fallback
export const getStaticProps: GetStaticProps = async () => {
  try {
    // Simulate data fetching
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) throw new Error('Failed to fetch data');
    const data = await response.json();
    return { props: { data: data.message }, revalidate: 10 }; // ISR
  } catch (error) {
    return { props: { error: error.message }, revalidate: 10 }; // Fallback
  }
};

export default ExamplePage;
```

**Comments:**
- **`getStaticProps`**: Handles data fetching with fallback in case of error.
- **ISR (Incremental Static Regeneration)**: Uses `revalidate` for periodic updates.
- **Graceful Degradation**: Displays error message or fallback content.