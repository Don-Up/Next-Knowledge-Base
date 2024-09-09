# Styling

## CSS Modules
### Scoped CSS with CSS Modules

**Example:**

```tsx
// styles/Home.module.css
.container {
  background-color: lightblue;
  padding: 20px;
}

// pages/index.tsx
import styles from '../styles/Home.module.css';

const HomePage = () => (
  <div className={styles.container}>
    <h1>Welcome to the Home Page</h1>
  </div>
);

export default HomePage;
```

**Comments:**

- **`styles/Home.module.css`**: Defines scoped CSS.
- **`import styles from '../styles/Home.module.css'`**: Imports CSS module.
- **`className={styles.container}`**: Applies scoped styles to elements



## Global Styles
### Using global.css

**Example:**

```tsx
// styles/global.css
body {
  margin: 0;
  font-family: Arial, sans-serif;
}

// pages/_app.tsx
import '../styles/global.css';

const MyApp = ({ Component, pageProps }) => (
  <Component {...pageProps} />
);

export default MyApp;
```

**Comments:**

- **`styles/global.css`**: Defines global CSS styles.
- **`import '../styles/global.css'`**: Imports global styles in `_app.tsx`.
- **`<Component {...pageProps} />`**: Renders all pages with global styles applied.



## Styled Components
### 1. CSS-in-JS with Styled Components

**Example:**

```tsx
// components/StyledButton.tsx
import styled from 'styled-components';

const StyledButton = styled.button`
  background-color: #0070f3;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;

  &:hover {
    background-color: #005bb5;
  }
`;

export default StyledButton;

// pages/index.tsx
import StyledButton from '../components/StyledButton';

const HomePage = () => (
  <div>
    <h1>Welcome to Next.js</h1>
    <StyledButton>Click Me</StyledButton>
  </div>
);

export default HomePage;
```

**Comments:**

- **`StyledButton`**: A button styled using `styled-components`.
- **`styled.button`**: Creates a styled `button` element with CSS-in-JS.
- **`<StyledButton>`**: Used in the page to render the styled button.

### 2. Integrating Emotion

**Example:**

```tsx
// pages/_app.tsx
import { CacheProvider } from '@emotion/react';
import createCache from '@emotion/cache';
import '../styles/globals.css';

const cache = createCache({ key: 'css', prepend: true });

function MyApp({ Component, pageProps }) {
  return (
    <CacheProvider value={cache}>
      <Component {...pageProps} />
    </CacheProvider>
  );
}

export default MyApp;

// components/StyledButton.tsx
/** @jsxImportSource @emotion/react */
import { css } from '@emotion/react';

const buttonStyle = css`
  background-color: #0070f3;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;

  &:hover {
    background-color: #005bb5;
  }
`;

const StyledButton = () => <button css={buttonStyle}>Click Me</button>;

export default StyledButton;

// pages/index.tsx
import StyledButton from '../components/StyledButton';

const HomePage = () => (
  <div>
    <h1>Welcome to Next.js</h1>
    <StyledButton />
  </div>
);

export default HomePage;
```

**Comments:**

- **`CacheProvider`**: Sets up Emotion's cache for CSS-in-JS.
- **`css`**: Emotion's function to create styled CSS blocks.
- **`StyledButton`**: A button styled with Emotion's `css` prop.



## Tailwind CSS
### 1. Setting Up Tailwind CSS

### 2. Utility-First Styling