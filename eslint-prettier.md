### husky init
```sh
npx husky-init && npm install
```

### npm install (eslint, prettier, lint-staged)

```sh
npm install --save-dev @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint eslint-config-prettier prettier lint-staged eslint-plugin-react eslint-plugin-react-hooks
```

#### end of package.json
```json
  "lint-staged": {
    "*.{js,jsx,ts,tsx,md,html,css}": "prettier --write",
    "**/*.{js,jsx,ts,tsx}": [
      "eslint"
    ]
  }
```

if using jest then 
```json
    "lint-staged": {
        "*.{js,jsx,ts,tsx,md,html,css}": "prettier --write",
        "**/*.{js,jsx,ts,tsx}": [
            "eslint",
            "jest --bail --findRelatedTests"
        ]
    }
```

#### .husky/pre-commit
```sh
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

tsc
npx lint-staged
```

#### .eslintrc.json
```json
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 2018,
    "sourceType": "module"
  },
  "plugins": [
    "@typescript-eslint",
    "react-hooks"
  ],
  "extends": [
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
    "prettier"
  ],
  "rules": {
    "no-use-before-define": "off",
    "react/prop-types": "off",
    "import/no-anonymous-default-export": "off",
    "no-unused-vars": "off",
    "@typescript-eslint/no-unused-vars": "error"
  }
}
```

#### .prettierrc.json
```json
{
  "trailingComma": "es5",
  "tabWidth": 2,
  "semi": true,
  "singleQuote": true,
  "printWidth": 100,
  "arrowParens": "always"
}
```

#### Vitest related
https://vitest.dev/guide/#trying-vitest-online

```sh
# with npm
npm install -D vitest
```

```json
{
  "scripts": {
    "test": "vitest",
    "coverage": "vitest run --coverage",
  }
}
```

```ts
import { defineConfig } from 'vitest/config'

export default defineConfig({
  test: {
    // ...
  },
})
```

https://vitest.dev/guide/in-source.html
```ts
// src/index.ts

// the implementation
export function add(...args: number[]) {
  return args.reduce((a, b) => a + b, 0)
}

// in-source test suites
if (import.meta.vitest) {
  const { it, expect } = import.meta.vitest
  it('add', () => {
    expect(add()).toBe(0)
    expect(add(1)).toBe(1)
    expect(add(1, 2, 3)).toBe(6)
  })
}
```

```ts
// vite.config.ts
import { defineConfig } from 'vitest/config'

export default defineConfig({
+ define: {
+   'import.meta.vitest': 'undefined',
+ },
  test: {
    includeSource: ['src/**/*.{js,ts}']
  },
})
```

```ts
// tsconfig.json
{
  "compilerOptions": {
    "types": [
+     "vitest/importMeta"
    ]
  }
}
```
