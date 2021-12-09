### husky init
```sh
npx husky-init && npm install 
```

### npm install (eslint, prettier, lint-staged)

```sh
npm install --save-dev @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint eslint-config-prettier prettier lint-staged
```

#### .husky/pre-commit
```sh
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

tsc
npx lint-staged
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
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "no-use-before-define":"off",
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
