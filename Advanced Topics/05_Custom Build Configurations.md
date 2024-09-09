# Custom Build Configurations

## Modifying webpack and Babel
### Extending Next.js Configuration

Extend Next.js build configurations using `next.config.js` to customize Webpack and Babel settings.

**Example:**

1. **Install Dependencies**:
   ```bash
   npm install --save-dev @babel/preset-env @babel/preset-react
   ```

2. **Configure `next.config.js`**:
   ```ts
   // next.config.js
   const path = require('path');
   
   module.exports = {
     webpack: (config, { isServer }) => {
       // Custom Webpack configuration
       config.resolve.alias['@components'] = path.join(__dirname, 'components');
       
       // Add custom Webpack plugins if needed
       if (!isServer) {
         config.plugins.push(new MyCustomWebpackPlugin());
       }
   
       return config;
     },
     babel: {
       presets: ['next/babel', '@babel/preset-env', '@babel/preset-react'],
       plugins: ['@babel/plugin-proposal-class-properties']
     },
   };
   ```

3. **Custom Babel Configuration** (optional, in `.babelrc`):
   ```json
   {
     "presets": ["next/babel", "@babel/preset-env", "@babel/preset-react"],
     "plugins": ["@babel/plugin-proposal-class-properties"]
   }
   ```

This setup allows you to extend Webpack for additional customization and configure Babel for advanced JavaScript features.