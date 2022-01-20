# How to convert a CRA project to Vite

## Update package.json

Modify your `package.json` scripts to use Vite:

```json
"scripts": {
  "start": "vite",
  "build": "vite build",
}
```

Make sure you delete `react-scripts` from your dependencies section - we don't need it anymore. \
Add the following devDependencies for Vite itself, as well as the React plugin:

```json
"devDependencies": {
  "@vitejs/plugin-react": "^1.0.7",
  "vite": "^2.7.2"
}
```

Then run `npm install`. \
Alternatively, run `npm install -D vite @vitejs/plugin-react`.

## Create a Vite config

Create a `vite.config.json` file in the root of your project. \
In the `plugins` section, add the `react()` plugin to use React.

```js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  build: { outDir: 'build' },
  envPrefix: 'REACT_APP_',
});
```

You may optionally configure several options to make Vite act more like CRA:

- Vite's `build` script will build to the `dist` folder by default. You may change it to `build` to be more similar to CRA.
- Vite only loads environment variables with the `VITE_` prefix. You may change this to `REACT_APP_` to match the prefix CRA uses.

## Move & modify your index.html

CRA loads the `index.html` file from the `public` folder, while Vite loads it from the project root. For this reason, move your `index.html` out of the `public` folder.

In addition, make the following changes to your `index.html`:

- Remove the `%PUBLIC_URL%` from anywhere it appears in the file.

```html
<!-- For example, this... -->
<link rel="icon" href="%PUBLIC_URL%/favicon.ico" />

<!-- ...should turn into this. -->
<link rel="icon" href="/favicon.ico" />
```

- Add a script tag to load the React code in the page.

```html
<script type="module" src="/src/index.jsx" />
```

## Rename `.js` files to `.jsx`

If you try to import `index.js` in the script tag above, you will get an error. This file, and any other files that have JSX, need to have their file extension changed to `.jsx`. \
For example, rename `index.js` to `index.jsx`, `App.js` to `App.jsx` and so on.

## Environment variables

If you use any environment variables in your project, you need to change every instance of `process.env.` to `import.meta.env.`

If you chose not to change the `envPrefix` in your Vite config, you have to rename your environment variables so they start with the `VITE_` prefix.
