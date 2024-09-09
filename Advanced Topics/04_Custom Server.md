# Custom Server

## Creating a Custom Express Server
### Integrating Next.js with Express or Koa

To customize server behavior, integrate Next.js with an Express server.

**Example:**

1. **Install Dependencies**:
   ```bash
   npm install express next react react-dom
   ```

2. **Create Custom Server** (e.g., in `server.ts`):
   ```ts
   import express from 'express';
   import next from 'next';
   
   const dev = process.env.NODE_ENV !== 'production';
   const app = next({ dev });
   const handle = app.getRequestHandler();
   
   app.prepare().then(() => {
     const server = express();
   
     // Custom route
     server.get('/custom', (req, res) => {
       return res.send('Custom route');
     });
   
     // Handle all other requests with Next.js
     server.all('*', (req, res) => {
       return handle(req, res);
     });
   
     server.listen(3000, () => {
       console.log('Server is running on http://localhost:3000');
     });
   });
   ```

3. **Update `package.json`** to start the custom server:
   ```json
   "scripts": {
     "dev": "ts-node server.ts",
     "build": "next build",
     "start": "NODE_ENV=production node server.js"
   }
   ```

This setup uses Express to handle custom routes and delegates other requests to Next.js.