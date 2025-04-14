# Installaing & configuring tailwind
All commands are run from the root of the project folder

- Install tailwind
```bash
npm install tailwindcss @tailwindcss/vite
```

- Configure vite config
```bash
code vite.config.js
```
- Add the following to the top of the file
```js
import tailwindcss from '@tailwindcss/vite'
```

- Modify the plugins property in the defineConfig object to the following
```js
export default defineConfig({
  plugins: [react(), tailwindcss()],
})
```
- Save and close the file

- Import the tailwind css styles to your main stylesheet
```bash
code src\index.css
```
- Add the following code to the top fo the file
```js
@import "tailwindcss";
```
- Save and clsoe the file
