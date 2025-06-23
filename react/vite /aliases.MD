# Vite Aliases

The following guide allows you to setup aliases within vite.

Are you tired of importing your customm oibraries and components like so?

../../../components/Card/CardList

Would you prefer to do it like this?

@components/Card/CardList

This guide will show you how

## Setting the config

* Open your vite.config.js file with your favourite editor
* At the top of the file add the following

```bash
import path from 'node:path'
```

* inside the defineConfig object add a new property like so it now looks like
```bash
export default defineConfig({
  plugins: [react(),tailwindcss()],
  resolve: {
    alias: {
      '@': path(__dirname, './src'),
      '@components': path(__dirname, './src/components'),
      '@features': path(__dirname, './src/features'),
      '@hooks': path(__dirname, './src/hooks'),
      '@services': path(__dirname, './src/services'),
      '@stores': path(__dirname, './src/stores'),
    }
  }
})
```

* Install the types for node to stop issues with vscode, path and __dirname
```bash
npm install -D @types/node
```
