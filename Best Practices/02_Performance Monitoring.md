# Performance Monitoring

## Using Lighthouse and Web Vitals
### Monitoring and Improving Performance

Monitor and improve performance with Lighthouse and Web Vitals in Next.js. Lighthouse provides detailed audits, while Web Vitals track key performance metrics.

**Example Setup:**

1. **Add `web-vitals` for measuring performance:**

   ```bash
   npm install web-vitals
   ```

2. **Integrate Web Vitals in `_app.tsx`:**

   ```tsx
   // pages/_app.tsx
   import { AppProps } from 'next/app';
   import { reportWebVitals } from '../lib/reportWebVitals';

   function MyApp({ Component, pageProps }: AppProps) {
     return <Component {...pageProps} />;
   }

   export default MyApp;

   // lib/reportWebVitals.ts
   import { ReportHandler } from 'web-vitals';

   export function reportWebVitals(metric: ReportHandler) {
     console.log(metric);
     // Send metrics to an analytics endpoint or logging service
   }
   ```

3. **Run Lighthouse in Chrome DevTools:**
   - Open Chrome DevTools (`F12` or `Ctrl+Shift+I`).
   - Navigate to the "Lighthouse" tab.
   - Click "Generate report" to analyze performance.

**Code Example:**

```tsx
// lib/reportWebVitals.ts
import { ReportHandler } from 'web-vitals';

export function reportWebVitals(metric: ReportHandler) {
  console.log(metric);  // Logs metrics like FCP, LCP, CLS
  // Example: Send metrics to a logging service
  fetch('/api/metrics', {
    method: 'POST',
    body: JSON.stringify(metric),
    headers: {
      'Content-Type': 'application/json'
    }
  });
}
```

**Optimize Performance:**
- Use Lighthouse reports to identify and fix performance issues.
- Monitor Web Vitals to ensure good user experience.

This setup helps in tracking performance metrics and making necessary improvements to enhance user experience.