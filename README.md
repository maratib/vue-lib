# Vue 3 lib component

```bash

yarn create vite
# Create new Vue vite project by selecting Vue, Typescript

yarn add -D json @types/node vite-plugin-dts
# Add above dependencies
```
```javascript
// Add following command to package.json scripts
"prepack": "json -f package.json -I -e \"delete this.devDependencies; delete this.dependencies\""
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

