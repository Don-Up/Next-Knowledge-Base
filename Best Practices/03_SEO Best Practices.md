# SEO Best Practices

## Ensuring Good SEO with Next.js
### Using Sitemap and robots.txt

Enhance SEO in Next.js by generating a sitemap and configuring `robots.txt` to guide search engine indexing.

**Example Setup:**

1. **Generate Sitemap with `next-sitemap`:**

   ```bash
   npm install next-sitemap
   ```

2. **Configure `next-sitemap`:**

   ```js
   // next-sitemap.js
   module.exports = {
     siteUrl: 'https://your-domain.com',
     generateRobotsTxt: true, // Generate robots.txt
   };
   ```

3. **Update `next.config.js` to include `next-sitemap`:**

   ```js
   // next.config.js
   const { withSitemap } = require('next-sitemap');

   module.exports = withSitemap({
     reactStrictMode: true,
   });
   ```

4. **Create `robots.txt` file manually or let `next-sitemap` generate it:**

   ```txt
   # robots.txt
   User-agent: *
   Disallow: /private
   Sitemap: https://your-domain.com/sitemap.xml
   ```

**Code Example:**

```js
// next-sitemap.js
module.exports = {
  siteUrl: 'https://your-domain.com',
  generateRobotsTxt: true,  // Generates a robots.txt file
  robotsTxtOptions: {
    additionalSitemaps: [
      'https://your-domain.com/my-custom-sitemap.xml',
    ],
  },
};
```

**Deploy and Verify:**
- Ensure `sitemap.xml` and `robots.txt` are correctly deployed.
- Use tools like Google Search Console to verify and submit your sitemap.

By setting up a sitemap and `robots.txt`, you guide search engines on indexing and crawling, improving your site's SEO.