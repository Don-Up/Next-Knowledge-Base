# Deployment

## Deploying on Vercel
### Optimizing for Vercel Platform

**Example:**

```ts
// vercel.json
{
  "builds": [
    { "src": "next.config.js", "use": "@vercel/next" }
  ],
  "routes": [
    { "src": "/api/(.*)", "dest": "/api/$1" },
    { "src": "/(.*)", "dest": "/$1" }
  ],
  "rewrites": [
    { "source": "/old-path/:slug*", "destination": "/new-path/:slug*" }
  ]
}
```

**Comments:**
- **`vercel.json`**: Configuration for Vercel deployment.
- **`builds`**: Specifies Next.js build setup.
- **`routes`**: Maps API and page routes.
- **`rewrites`**: Redirects old paths to new ones.



## Other Deployment Options
### AWS, Azure, Google Cloud

**Next.js - Testing: Other Deployment Options - AWS, Azure, Google Cloud**

Deploy Next.js apps to AWS, Azure, or Google Cloud using their respective services:

- **AWS**: Use AWS Amplify or Elastic Beanstalk.
- **Azure**: Deploy via Azure Static Web Apps or Azure App Service.
- **Google Cloud**: Utilize Google Cloud Run or App Engine.

**Example:**

```sh
# AWS Amplify CLI
amplify init
amplify add hosting
amplify publish

# Azure CLI
az webapp create --resource-group <resource-group> --plan <app-service-plan> --name <app-name> --runtime "NODE|14-lts"

# Google Cloud CLI
gcloud app deploy
```



## Environment Variables
### Managing Secrets and Configurations

Use `.env.local` for local secrets and configurations. Ensure these are excluded from version control. Access variables in code via `process.env.VAR_NAME`.

**Example:**

```js
// .env.local
NEXT_PUBLIC_API_URL=https://api.example.com

// In your component
const apiUrl = process.env.NEXT_PUBLIC_API_URL;
```

For deployment, configure environment variables in your hosting platform’s settings.