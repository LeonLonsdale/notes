# Installation and Setup

## Installation

```bash
npx create-next-app@latest

Would you like to use TypeScript? 'Yes'
Would you like to use ESLint? 'Yes'
Would you like to use Tailwind CSS? 'Yes'
Would you like to use 'src/' directory? 'Yes'
Would you like to use App Router? 'Yes'
Would you like to customise the default import alias (@/*)? 'No'
```

[Back to contents](../README.md)

## Add Prettier and Prettier for Tailwind and ESLint

```bash
npm i -D prettier prettier-plugin-tailwindcss
```

Create Prettier config

```bash
touch .prettierrc
```

Add your prettier settings. These are my preferred settings below:

```json
{
  "printWidth": 80,
  "tabWidth": 2,
  "semi": true,
  "singleQuote": true,
  "jsxSingleQuote": true,
  "trailingComma": "all",
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "always",
  "overrides": [
    {
      "files": ["*.html"],
      "options": {
        "tabWidth": 4,
        "semi": false,
        "trailingComma": "none"
      }
    }
  ]
}
```

Add tailwind css plugin to config

```json
{
  "printWidth": 80,
  "tabWidth": 2,
  "semi": true,
  "singleQuote": true,
  "jsxSingleQuote": true,
  "trailingComma": "all",
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "always",
  "overrides": [
    {
      "files": ["*.html"],
      "options": {
        "tabWidth": 4,
        "semi": false,
        "trailingComma": "none"
      }
    }
  ],
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

Let ESLint know about Prettier in `.eslintrc.json`

```json
{
  "extends": ["next/core-web-vitals", "prettier"]
}
```

[Back to Contents](../README.md)

## Enable WordWrapping and set default formatter in VS Code

1. Install the Prettier extension
2. Create a new folder in root `.vscode`
3. Inside create `settings.json`

```bash
mkdir .vscode
touch .vscode/settings.json
```

In the settings json, add

```json
{
  "editor.wordWrap": "wordWrapColumn",
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.wordWrapColumn": 80
}
```

[Back to Contents](../README.md)
