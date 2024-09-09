# WebSockets and Real-time Data

##  Setting Up WebSockets
### Using Libraries like Socket.IO

To enable real-time data, integrate `Socket.IO` with Next.js. Use `socket.io` for communication between server and client.

**Example:**

1. **Install Dependencies**:
   ```bash
   npm install socket.io socket.io-client
   ```

2. **Server Setup** (e.g., in `server.ts`):
   ```ts
   import { Server } from 'socket.io';
   import http from 'http';
   
   const server = http.createServer();
   const io = new Server(server);
   
   io.on('connection', (socket) => {
     console.log('a user connected');
     socket.on('disconnect', () => {
       console.log('user disconnected');
     });
   });
   
   server.listen(3000, () => console.log('Server running on port 3000'));
   ```

3. **Client Setup** (e.g., in `pages/index.tsx`):
   ```tsx
   import { useEffect } from 'react';
   import io from 'socket.io-client';
   
   const socket = io('http://localhost:3000');
   
   const Home = () => {
     useEffect(() => {
       socket.on('connect', () => {
         console.log('connected to server');
       });
   
       return () => {
         socket.disconnect();
       };
     }, []);
   
     return <div>Welcome to Next.js with WebSockets</div>;
   };
   
   export default Home;
   ```

In this setup, `socket.io` handles real-time communication, with the server managing connections and the client subscribing to events.