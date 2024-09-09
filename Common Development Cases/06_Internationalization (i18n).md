# Internationalization (i18n)

## Configuring i18n in Next.js
### 1. Localized Routes

**Example:**

```tsx
// next.config.js
module.exports = {
  i18n: {
    locales: ['en', 'fr'],
    defaultLocale: 'en',
  },
};

// pages/index.tsx
import { useRouter } from 'next/router';

const HomePage = () => {
  const { locale, asPath } = useRouter();

  return (
    <div>
      <h1>
        {locale === 'fr' ? 'Bienvenue sur Next.js' : 'Welcome to Next.js'}
      </h1>
      <p>
        <a href={asPath} locale={locale === 'en' ? 'fr' : 'en'}>
          {locale === 'fr' ? 'Switch to English' : 'Passer au français'}
        </a>
      </p>
    </div>
  );
};

export default HomePage;
```

**Comments:**

- **`i18n`**: Configures locales and default locale in `next.config.js`.
- **`useRouter`**: Retrieves locale and current path for localization.
- **Localized Routes**: Allows switching between languages in links.

### 2. Language Detection and Redirects

**Example:**

```tsx
// next.config.js
const NextI18Next = require('next-i18next').default;

module.exports = {
  i18n: {
    locales: ['en', 'fr'],
    defaultLocale: 'en',
    localeDetection: true,
  },
};

// pages/_middleware.ts
import { NextRequest, NextResponse } from 'next/server';

export function middleware(req: NextRequest) {
  const { pathname, nextUrl } = req;
  const userLocale = req.headers.get('accept-language')?.includes('fr') ? 'fr' : 'en';

  if (pathname === '/') {
    return NextResponse.redirect(new URL(`/${userLocale}`, req.url));
  }
  
  return NextResponse.next();
}
```

**Comments:**

- **`i18n`**: Configures locales and enables locale detection in `next.config.js`.
- **`_middleware.ts`**: Detects user language and redirects to the localized route based on `accept-language` header.



## Managing Translations
### JSON Files or i18next

**Example:**

```tsx
// next.config.js
const { i18n } = require('./next-i18next.config');
module.exports = {
  i18n,
};

// next-i18next.config.js
module.exports = {
  i18n: {
    locales: ['en', 'fr'],
    defaultLocale: 'en',
  },
  localePath: './public/locales',
};

// public/locales/en/translation.json
{
  "welcome": "Welcome to our website!"
}

// public/locales/fr/translation.json
{
  "welcome": "Bienvenue sur notre site Web!"
}

// pages/index.tsx
import { useTranslation } from 'next-i18next';

export default function Home() {
  const { t } = useTranslation();

  return <h1>{t('welcome')}</h1>;
}
```

**Comments:**
- **`next.config.js`**: Sets up i18n configuration.
- **`next-i18next.config.js`**: Specifies locales and paths for translation files.
- **`translation.json`**: Contains translations for different languages.
- **`useTranslation`**: Hook to fetch and use translations in components.



## Handling SEO for Multiple Languages
### Managing Meta Tags and Canonicals

**Example:**

```tsx
// pages/_app.tsx
import { useRouter } from 'next/router';
import Head from 'next/head';

const MyApp = ({ Component, pageProps }) => {
  const { locale } = useRouter();

  const getMetaTags = (locale: string) => {
    switch (locale) {
      case 'fr':
        return { title: 'Page d\'accueil', description: 'Bienvenue sur notre site Web' };
      case 'en':
      default:
        return { title: 'Home Page', description: 'Welcome to our website' };
    }
  };

  const { title, description } = getMetaTags(locale || 'en');

  return (
    <>
      <Head>
        <title>{title}</title>
        <meta name="description" content={description} />
        <link rel="canonical" href={`https://example.com/${locale}`} />
      </Head>
      <Component {...pageProps} />
    </>
  );
};

export default MyApp;
```

**Comments:**
- **`_app.tsx`**: Custom App component where meta tags are set based on locale.
- **`getMetaTags`**: Function to provide different meta tags based on language.
- **`<Head>`**: Sets title, description, and canonical URL for SEO.