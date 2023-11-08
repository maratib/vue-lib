# Vue 3 lib components
This is to create custom Vue components library for publishing as NPM package.
```bash

yarn create vite
# Create new Vue vite project by selecting Vue, Typescript

yarn add -D json @types/node vite-plugin-dts
# Add above dependencies
```
```javascript
// Add following command to package.json scripts
"prepack": "json -f package.json -I -e \"delete this.devDependencies; delete this.dependencies\"",
 "npm-pack": "cp package.json package1.json && npm pack && rm package.json && cp package1.json package.json && rm package1.json"

// Adding to following to package.json
"main": "./dist/my-lib.umd.js",
"module": "./dist/my-lib.es.js",
"types": "./dist/index.d.ts",
"files": [
    "dist"
  ],
"peerDependencies": {
    "vue": "^3.3.4"
  },
  "exports": {
    ".": {
      "import": "./dist/my-lib.es.js",
      "require": "./dist/my-lib.umd.js"
    }
  },
```

## update vite.config.ts
```javascript
import vue from '@vitejs/plugin-vue'
import path from 'node:path';
import { defineConfig } from 'vite';
import dts from 'vite-plugin-dts';

export default defineConfig({
  plugins: [
    vue(),
    dts({
      insertTypesEntry: true,
    }),
  ],
  build: {
    sourcemap: true,
    lib: {
      entry: path.resolve(__dirname, 'src/components/index.ts'),
      name: 'MyLib',
      formats: ['es', 'umd'],
      fileName: (format) => `my-lib.${format}.js`,
    },
    rollupOptions: {
      external: ['vue'],
      output: {
        globals: {
          vue: 'Vue',
        },
      },
    },
  },
});
```
```bash
# Run the following command to build and package NPM
yarn build
yarn npm-pack
