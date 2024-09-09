# Data Fetching

## getStaticProps (SSG)
### 1. Fetching Data at Build Time

`getStaticProps` is used for Static Site Generation (SSG), fetching data at build time to pre-render static pages.

**Example:**

1. **Page Component with `getStaticProps`:**
   
   - File: `pages/index.tsx`
   ```tsx
   // pages/index.tsx
   import { GetStaticProps } from 'next';
   
   // Define the props interface
   interface Props {
     data: string[];
   }
   
   // Component receiving props
   const HomePage = ({ data }: Props) => (
     <div>
       <h1>Static Data</h1>
       <ul>
         {data.map((item, index) => (
           <li key={index}>{item}</li>
         ))}
       </ul>
     </div>
   );
   
   // Fetch data at build time
   export const getStaticProps: GetStaticProps<Props> = async () => {
     // Simulate fetching data from an API or database
     const fetchData = async () => {
       return new Promise<string[]>((resolve) => {
         setTimeout(() => resolve(['Item 1', 'Item 2', 'Item 3']), 1000);
       });
     };
   
     const data = await fetchData();
   
     // Return the data as props
     return {
       props: {
         data,
       },
       revalidate: 10, // Optional: revalidate every 10 seconds
     };
   };
   
   export default HomePage;
   ```

**Comments:**
- **`getStaticProps`**: Fetches data at build time for static generation.
- **Data Fetching**: `fetchData` simulates an asynchronous data fetch.
- **`revalidate`**: Optionally specifies a revalidation interval for incremental static regeneration.
- **Static Props**: Returns the fetched data as props to the page component for rendering.

### 2. Generating Static Pages

`getStaticProps` fetches data at build time for static page generation.

**Example:**

1. **Page Component with `getStaticProps`:**
   - File: `pages/products.tsx`
   ```tsx
   // pages/products.tsx
   import { GetStaticProps } from 'next';
   
   // Define the props interface
   interface Product {
     id: number;
     name: string;
   }
   
   interface Props {
     products: Product[];
   }
   
   // Page component displaying product list
   const ProductsPage = ({ products }: Props) => (
     <div>
       <h1>Products</h1>
       <ul>
         {products.map((product) => (
           <li key={product.id}>{product.name}</li>
         ))}
       </ul>
     </div>
   );
   
   // Fetch data at build time
   export const getStaticProps: GetStaticProps<Props> = async () => {
     // Simulate fetching data from an API
     const fetchProducts = async (): Promise<Product[]> => {
       return new Promise((resolve) => {
         setTimeout(() => resolve([
           { id: 1, name: 'Product A' },
           { id: 2, name: 'Product B' },
           { id: 3, name: 'Product C' }
         ]), 1000);
       });
     };
   
     const products = await fetchProducts();
   
     // Return data as props
     return {
       props: {
         products,
       },
     };
   };
   
   export default ProductsPage;
   ```

**Comments:**
- **`getStaticProps`**: Fetches data at build time for static page generation.
- **Data Fetching**: `fetchProducts` simulates fetching from an API.
- **Static Props**: Returns the fetched data as props for static generation.



## getServerSideProps (SSR)
### 1. Fetching Data on Each Request

`getServerSideProps` fetches data on each request for server-side rendering.

**Example:**

1. **Page Component with `getServerSideProps`:**
   - File: `pages/posts.tsx`
   ```tsx
   // pages/posts.tsx
   import { GetServerSideProps } from 'next';
   
   // Define the props interface
   interface Post {
     id: number;
     title: string;
   }
   
   interface Props {
     posts: Post[];
   }
   
   // Page component displaying a list of posts
   const PostsPage = ({ posts }: Props) => (
     <div>
       <h1>Posts</h1>
       <ul>
         {posts.map((post) => (
           <li key={post.id}>{post.title}</li>
         ))}
       </ul>
     </div>
   );
   
   // Fetch data on each request
   export const getServerSideProps: GetServerSideProps<Props> = async () => {
     // Simulate fetching data from an API
     const fetchPosts = async (): Promise<Post[]> => {
       return new Promise((resolve) => {
         setTimeout(() => resolve([
           { id: 1, title: 'Post 1' },
           { id: 2, title: 'Post 2' },
           { id: 3, title: 'Post 3' }
         ]), 1000);
       });
     };
   
     const posts = await fetchPosts();
   
     // Return data as props
     return {
       props: {
         posts,
       },
     };
   };
   
   export default PostsPage;
   ```

**Comments:**
- **`getServerSideProps`**: Fetches data on each request for server-side rendering.
- **Data Fetching**: `fetchPosts` simulates fetching from an API on each request.
- **Server-Side Props**: Returns the fetched data as props for server-side rendering.

### 2. Handling Server-Side Rendering

**Next.js - Data Fetching with `getServerSideProps` (SSR): Handling Server-Side Rendering**

`getServerSideProps` enables server-side rendering by fetching data on each request.

**Example:**

1. **Page Component with `getServerSideProps`:**
   - File: `pages/products.tsx`
   ```tsx
   // pages/products.tsx
   import { GetServerSideProps } from 'next';
   
   // Define the props interface
   interface Product {
     id: number;
     name: string;
   }
   
   interface Props {
     products: Product[];
   }
   
   // Page component displaying a list of products
   const ProductsPage = ({ products }: Props) => (
     <div>
       <h1>Products</h1>
       <ul>
         {products.map((product) => (
           <li key={product.id}>{product.name}</li>
         ))}
       </ul>
     </div>
   );
   
   // Fetch data on each request
   export const getServerSideProps: GetServerSideProps<Props> = async () => {
     // Simulate fetching data from an API
     const fetchProducts = async (): Promise<Product[]> => {
       return new Promise((resolve) => {
         setTimeout(() => resolve([
           { id: 1, name: 'Product A' },
           { id: 2, name: 'Product B' },
           { id: 3, name: 'Product C' }
         ]), 1000);
       });
     };
   
     const products = await fetchProducts();
   
     // Return data as props
     return {
       props: {
         products,
       },
     };
   };
   
   export default ProductsPage;
   ```

**Comments:**
- **`getServerSideProps`**: Fetches data on each request for server-side rendering.
- **Data Fetching**: `fetchProducts` simulates fetching from an API on each request.
- **Server-Side Props**: Provides fetched data to the component for SSR.



## getStaticPaths
### 1. Dynamic SSG with getStaticPaths

`getStaticPaths` generates dynamic static pages by defining which paths to pre-render at build time.

**Example:**

1. **Dynamic Page with `getStaticPaths` and `getStaticProps`:**
   - File: `pages/products/[id].tsx`
   ```tsx
   // pages/products/[id].tsx
   import { GetStaticPaths, GetStaticProps } from 'next';
   
   interface Product {
     id: number;
     name: string;
   }
   
   interface Props {
     product: Product;
   }
   
   // Page component displaying a single product
   const ProductPage = ({ product }: Props) => (
     <div>
       <h1>{product.name}</h1>
       <p>ID: {product.id}</p>
     </div>
   );
   
   // Fetch paths for dynamic routes at build time
   export const getStaticPaths: GetStaticPaths = async () => {
     // Simulate fetching product IDs
     const fetchProductIds = async (): Promise<number[]> => {
       return new Promise((resolve) => {
         setTimeout(() => resolve([1, 2, 3]), 1000);
       });
     };
   
     const ids = await fetchProductIds();
   
     // Generate paths for all products
     const paths = ids.map(id => ({ params: { id: id.toString() } }));
   
     return {
       paths,
       fallback: false, // Show 404 for paths not returned by getStaticPaths
     };
   };
   
   // Fetch data for each path at build time
   export const getStaticProps: GetStaticProps<Props> = async (context) => {
     const { id } = context.params!;
   
     // Simulate fetching product data
     const fetchProduct = async (id: number): Promise<Product> => {
       return new Promise((resolve) => {
         setTimeout(() => resolve({ id, name: `Product ${id}` }), 1000);
       });
     };
   
     const product = await fetchProduct(Number(id));
   
     return {
       props: {
         product,
       },
     };
   };
   
   export default ProductPage;
   ```

**Comments:**
- **`getStaticPaths`**: Defines paths to pre-render at build time.
- **Dynamic Routes**: `[id]` allows for parameterized paths.
- **Data Fetching**: `fetchProductIds` and `fetchProduct` simulate data retrieval.

### 2. Pre-rendering Dynamic Routes

`getStaticPaths` generates paths for dynamic routes to be pre-rendered at build time.

**Example:**

1. **Dynamic Page with `getStaticPaths` and `getStaticProps`:**
   - File: `pages/posts/[slug].tsx`
   ```tsx
   // pages/posts/[slug].tsx
   import { GetStaticPaths, GetStaticProps } from 'next';
   
   interface Post {
     slug: string;
     title: string;
   }
   
   interface Props {
     post: Post;
   }
   
   // Page component displaying a post
   const PostPage = ({ post }: Props) => (
     <div>
       <h1>{post.title}</h1>
       <p>Slug: {post.slug}</p>
     </div>
   );
   
   // Generate paths for dynamic routes at build time
   export const getStaticPaths: GetStaticPaths = async () => {
     // Simulate fetching post slugs
     const fetchPostSlugs = async (): Promise<string[]> => {
       return new Promise((resolve) => {
         setTimeout(() => resolve(['post-1', 'post-2']), 1000);
       });
     };
   
     const slugs = await fetchPostSlugs();
   
     // Generate paths for all posts
     const paths = slugs.map(slug => ({ params: { slug } }));
   
     return {
       paths,
       fallback: false, // Show 404 for paths not returned by getStaticPaths
     };
   };
   
   // Fetch data for each path at build time
   export const getStaticProps: GetStaticProps<Props> = async (context) => {
     const { slug } = context.params!;
   
     // Simulate fetching post data
     const fetchPost = async (slug: string): Promise<Post> => {
       return new Promise((resolve) => {
         setTimeout(() => resolve({ slug, title: `Title of ${slug}` }), 1000);
       });
     };
   
     const post = await fetchPost(slug);
   
     return {
       props: {
         post,
       },
     };
   };
   
   export default PostPage;
   ```

**Comments:**
- **`getStaticPaths`**: Generates paths for dynamic routes at build time.
- **Dynamic Routes**: `[slug]` allows parameterized URLs.
- **Data Fetching**: Simulated data fetching with `fetchPostSlugs` and `fetchPost`.



## Client-side Data Fetching
### 1. Fetching Data with useEffect

Client-side data fetching is done using React hooks like `useEffect` for data that doesn't need to be pre-rendered.

**Example:**

1. **Component Fetching Data with `useEffect`:**
   - File: `pages/client-side.tsx`
   ```tsx
   // pages/client-side.tsx
   import { useEffect, useState } from 'react';
   
   interface Data {
     id: number;
     title: string;
   }
   
   const ClientSidePage = () => {
     const [data, setData] = useState<Data | null>(null);
     const [loading, setLoading] = useState<boolean>(true);
     const [error, setError] = useState<string | null>(null);
   
     useEffect(() => {
       // Fetch data on component mount
       const fetchData = async () => {
         try {
           const response = await fetch('https://jsonplaceholder.typicode.com/posts/1');
           if (!response.ok) throw new Error('Network response was not ok');
           const data = await response.json();
           setData(data);
         } catch (error) {
           setError(error.message);
         } finally {
           setLoading(false);
         }
       };
   
       fetchData();
     }, []); // Empty dependency array means this runs once on mount
   
     if (loading) return <p>Loading...</p>;
     if (error) return <p>Error: {error}</p>;
   
     return (
       <div>
         <h1>{data?.title}</h1>
         <p>ID: {data?.id}</p>
       </div>
     );
   };
   
   export default ClientSidePage;
   ```

**Comments:**
- **`useEffect`**: Fetches data client-side after component mounts.
- **State Management**: Uses `useState` for data, loading, and error states.
- **Data Fetching**: `fetchData` handles async data fetching.

### 2. Using SWR or React Query

Use libraries like SWR or React Query for efficient client-side data fetching with caching and revalidation.

**Example with SWR:**

1. **Component Using SWR:**
   - File: `pages/client-side-swr.tsx`
   ```tsx
   // pages/client-side-swr.tsx
   import useSWR from 'swr';
   
   const fetcher = (url: string) => fetch(url).then(res => res.json());
   
   const ClientSideSWRPage = () => {
     const { data, error } = useSWR('https://jsonplaceholder.typicode.com/posts/1', fetcher);
   
     if (error) return <p>Error: {error.message}</p>;
     if (!data) return <p>Loading...</p>;
   
     return (
       <div>
         <h1>{data.title}</h1>
         <p>ID: {data.id}</p>
       </div>
     );
   };
   
   export default ClientSideSWRPage;
   ```

**Example with React Query:**

1. **Component Using React Query:**
   - File: `pages/client-side-query.tsx`
   ```tsx
   // pages/client-side-query.tsx
   import { useQuery } from 'react-query';
   
   const fetchPost = async () => {
     const response = await fetch('https://jsonplaceholder.typicode.com/posts/1');
     if (!response.ok) throw new Error('Network response was not ok');
     return response.json();
   };
   
   const ClientSideQueryPage = () => {
     const { data, error, isLoading } = useQuery('post', fetchPost);
   
     if (isLoading) return <p>Loading...</p>;
     if (error instanceof Error) return <p>Error: {error.message}</p>;
   
     return (
       <div>
         <h1>{data?.title}</h1>
         <p>ID: {data?.id}</p>
       </div>
     );
   };
   
   export default ClientSideQueryPage;
   ```

**Comments:**
- **SWR/React Query**: Simplify client-side data fetching with features like caching and revalidation.
- **Fetcher**: For SWR, define a `fetcher` function to fetch data.
- **Queries**: Use `useQuery` for React Query to handle async data fetching.