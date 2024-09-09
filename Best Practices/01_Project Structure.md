# Project Structure

## Organizing Folders and Files
### Pages, Components, Lib, Public

Organize a Next.js project with clear folder structure: `pages` for routes, `components` for reusable UI elements, `lib` for utilities, and `public` for static assets.

**Example Structure:**
```
/my-next-app
|-- /components    # Reusable UI components
|   |-- Header.tsx
|   |-- Footer.tsx
|-- /lib            # Utility functions
|   |-- api.ts
|-- /pages          # Route files
|   |-- index.tsx   # Home page
|   |-- about.tsx   # About page
|-- /public         # Static assets (images, etc.)
|   |-- logo.png
|-- /styles         # Global styles
|   |-- globals.css
|-- next.config.js  # Next.js configuration
|-- tsconfig.json   # TypeScript configuration
|-- package.json    # Project metadata
```

**Code Example:**

1. **`pages/index.tsx`**:
   ```tsx
   // pages/index.tsx
   import Header from '../components/Header';
   import Footer from '../components/Footer';
   
   const HomePage = () => (
     <div>
       <Header />
       <h1>Welcome to My Next.js App</h1>
       <Footer />
     </div>
   );
   
   export default HomePage;
   ```

2. **`components/Header.tsx`**:
   ```tsx
   // components/Header.tsx
   const Header = () => (
     <header>
       <h1>My App Header</h1>
     </header>
   );
   
   export default Header;
   ```

3. **`lib/api.ts`**:
   ```ts
   // lib/api.ts
   export const fetchData = async (endpoint: string) => {
     const response = await fetch(endpoint);
     return response.json();
   };
   ```

4. **`public/logo.png`**: (Static file placed in `/public`)

This organization enhances maintainability and scalability by separating concerns effectively.