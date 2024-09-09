# API Routes

## Creating RESTful APIs

### Handling Requests and Responses

**Example:**

1. **API Route for CRUD Operations:**
   - File: `pages/api/items.ts`
   ```tsx
   // pages/api/items.ts
   import type { NextApiRequest, NextApiResponse } from 'next';
   
   type Item = {
     id: number;
     name: string;
   };
   
   // Mock database
   const items: Item[] = [{ id: 1, name: 'Item 1' }, { id: 2, name: 'Item 2' }];
   
   export default function handler(req: NextApiRequest, res: NextApiResponse) {
     if (req.method === 'GET') {
       // Handle GET request
       res.status(200).json(items);
     } else if (req.method === 'POST') {
       // Handle POST request
       const newItem: Item = req.body;
       items.push(newItem);
       res.status(201).json(newItem);
     } else {
       // Method Not Allowed
       res.setHeader('Allow', ['GET', 'POST']);
       res.status(405).end(`Method ${req.method} Not Allowed`);
     }
   }
   ```

**Comments:**
- **GET**: Retrieves data (items).
- **POST**: Adds new data to the list.
- **Method Handling**: Responds with appropriate status codes based on request type.
- **`NextApiRequest` and `NextApiResponse`**: Types for API request and response.



## Middleware in API Routes
### Using Middleware for Authentication

**Example:**

1. **Middleware Function:**
   - File: `middleware/auth.ts`
   ```ts
   // middleware/auth.ts
   import type { NextApiRequest, NextApiResponse } from 'next';
   
   export function authenticate(req: NextApiRequest, res: NextApiResponse, next: () => void) {
     const token = req.headers.authorization;
     if (token === 'valid-token') {
       next();
     } else {
       res.status(401).json({ message: 'Unauthorized' });
     }
   }
   ```

2. **API Route with Middleware:**
   - File: `pages/api/protected.ts`
   ```ts
   // pages/api/protected.ts
   import type { NextApiRequest, NextApiResponse } from 'next';
   import { authenticate } from '../../middleware/auth';
   
   export default function handler(req: NextApiRequest, res: NextApiResponse) {
     authenticate(req, res, () => {
       if (req.method === 'GET') {
         // Handle GET request
         res.status(200).json({ message: 'Success' });
       } else {
         // Method Not Allowed
         res.setHeader('Allow', ['GET']);
         res.status(405).end(`Method ${req.method} Not Allowed`);
       }
     });
   }
   ```

**Comments:**
- **Middleware Function**: Checks authentication token.
- **API Route**: Uses middleware to secure access.
- **`authenticate`**: Validates requests before proceeding.



## Error Handling
### Sending Error Responses

**Example:**

1. **API Route with Error Handling:**
   - File: `pages/api/example.ts`
   ```ts
   // pages/api/example.ts
   import type { NextApiRequest, NextApiResponse } from 'next';
   
   export default function handler(req: NextApiRequest, res: NextApiResponse) {
     try {
       if (req.method === 'POST') {
         const { data } = req.body;
         if (!data) throw new Error('Data is required');
         // Process data
         res.status(200).json({ message: 'Success' });
       } else {
         res.setHeader('Allow', ['POST']);
         res.status(405).json({ error: 'Method Not Allowed' });
       }
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   }
   ```

**Comments:**
- **Error Handling**: Wraps logic in `try-catch`.
- **Response Codes**: Sends `500` for server errors, `405` for method errors.
- **Error Messages**: Provides specific error details in JSON.



## Integrating with Databases
### Connecting to MongoDB

**Example:**

1. **Connecting to MongoDB:**
   - File: `lib/mongodb.ts`
   ```ts
   // lib/mongodb.ts
   import { MongoClient } from 'mongodb';
   
   const uri = process.env.MONGODB_URI as string;
   let client: MongoClient;
   
   export async function connectToDatabase() {
     if (client) return client.db();
     client = new MongoClient(uri, { useNewUrlParser: true, useUnifiedTopology: true });
     await client.connect();
     return client.db('mydatabase'); // Replace 'mydatabase' with your database name
   }
   ```

2. **API Route with MongoDB Integration:**
   - File: `pages/api/data.ts`
   ```ts
   // pages/api/data.ts
   import type { NextApiRequest, NextApiResponse } from 'next';
   import { connectToDatabase } from '../../lib/mongodb';
   
   export default async function handler(req: NextApiRequest, res: NextApiResponse) {
     const db = await connectToDatabase();
     try {
       if (req.method === 'GET') {
         const data = await db.collection('mycollection').find({}).toArray();
         res.status(200).json(data);
       } else {
         res.setHeader('Allow', ['GET']);
         res.status(405).json({ error: 'Method Not Allowed' });
       }
     } catch (error) {
       res.status(500).json({ error: 'Failed to fetch data' });
     }
   }
   ```

**Comments:**
- **Database Connection**: `connectToDatabase` establishes a connection.
- **API Route**: Fetches data from MongoDB and handles errors.
- **Environment Variables**: Uses `MONGODB_URI` for MongoDB connection.

### Connecting to PostgreSQL

**Example:**

1. **Connecting to PostgreSQL:**
   - File: `lib/postgres.ts`
   ```ts
   // lib/postgres.ts
   import { Client } from 'pg';
   
   const client = new Client({
     connectionString: process.env.DATABASE_URL,
   });
   
   export async function connectToDatabase() {
     if (!client._connected) await client.connect();
     return client;
   }
   ```

2. **API Route with PostgreSQL Integration:**
   - File: `pages/api/users.ts`
   ```ts
   // pages/api/users.ts
   import type { NextApiRequest, NextApiResponse } from 'next';
   import { connectToDatabase } from '../../lib/postgres';
   
   export default async function handler(req: NextApiRequest, res: NextApiResponse) {
     const client = await connectToDatabase();
     try {
       if (req.method === 'GET') {
         const result = await client.query('SELECT * FROM users');
         res.status(200).json(result.rows);
       } else {
         res.setHeader('Allow', ['GET']);
         res.status(405).json({ error: 'Method Not Allowed' });
       }
     } catch (error) {
       res.status(500).json({ error: 'Failed to fetch data' });
     }
   }
   ```

**Comments:**
- **Database Connection**: `connectToDatabase` establishes a connection to PostgreSQL.
- **API Route**: Fetches data from the `users` table and handles errors.
- **Environment Variables**: Uses `DATABASE_URL` for PostgreSQL connection.