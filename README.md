## Getting started

```bash
yarn start
```

### Editor config

Add file `.editorconfig`

```yaml
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = crlf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
```

### Prettier config

Add file `.prettierrc`

```json
{
  "printWidth": 120,
  "tabWidth": 2,
  "useTabs": false,
  "semi": false
}
```

### React

Add dependencies

```bash
yarn add react react-dom && yarn add -D @types/react @types/react-dom
```

Add compiler options to `tsconfig.json`

```json
"jsx": "react-jsx",
```

Add file `app/index.tsx`

```tsx
import { createRoot } from "react-dom/client"

// Clear the existing HTML content
document.body.innerHTML = '<div id="app"></div>'

// Render your React component instead
const root = createRoot(document.getElementById("app"))
root.render(<h1>Hello, Electron with React!</h1>)
```

Add import to `renderer.ts`

```ts
import "./app/index"
```

### Tailwindcss

Add dependencies

```bash
yarn add -D tailwindcss postcss-loader autoprefixer && npx tailwindcss init --ts -p
```

Add file `app/global.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Add import to `app/index.tsx`

```ts
import "./global.css"
```

Add `postcss-loader` configs to `webpack.renderer.config.ts`

```ts
rules.push({
  test: /\.css$/,
  use: [
    { loader: "style-loader" },
    { loader: "css-loader" },
    {
      loader: "postcss-loader",
      options: {
        postcssOptions: {
          plugins: [require("tailwindcss"), require("autoprefixer")],
        },
      },
    },
  ],
})
```

Config tailwindcss content path in `tailwindcss.config.ts`

```ts
import type { Config } from "tailwindcss"

export default {
  content: ["./src/**/*.{html,js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
} satisfies Config
```

### Shadcn/ui

```bash
npx shadcn-ui@latest init && npx shadcn-ui@latest add button
```

Add file `lin/utils.ts`

```ts
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}
```

Add dependencies to `package.json`

```bash
yarn add -D tsconfig-paths-webpack-plugin typescript-transform-paths
```

Update import path in `tsconfig.json`

```json
"paths": {
  "*": ["node_modules/*"],
  "@/*": ["./src/*"]
}
...
"plugins": [
  {
    "transform": "typescript-transform-paths",
    "afterDeclarations": true
  }
]
```

Update `webpack.renderer.config.ts`

```ts
import { TsconfigPathsPlugin } from "tsconfig-paths-webpack-plugin"
...
resolve: {
  extensions: [".js", ".ts", ".jsx", ".tsx", ".css"],
  plugins: [new TsconfigPathsPlugin()],
},
```

### ESLint config

````bash
yarn add -D eslint-import-resolver-typescript```
````

Add to `.eslintrc.json`

```json
"settings": {
  "import/resolver": {
    "typescript": {}
  }
}
```
