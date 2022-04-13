# CRA Recipe

This is a step-by-step guide to customize CRA for projects.

You will get an application which has;

* TypeScript
* Linting
* Formatting

## Table of Contents

* [Step 1: Creating a new app](#step-1-creating-a-new-app)
* [Step 2: Removing CRA example files](#step-2-removing-cra-example-files)
* [Step 3: VSCode settings](#step-3-vscode-settings)
* [Step 4: Make TypeScript more strict](#step-4-make-typescript-more-strict)
* [Step 5: Installing ESLint](#step-5-installing-eslint)
* [Step 6: Installing Prettier](#step-6-installing-prettier)

## Step 1: Creating a new app

First of all, we need to initialize our codebase via CRA command.

```sh
npx create-react-app react-starter --template typescript
cd cra-starter
yarn start
```

## Step 2: Removing CRA example files

In order to create our stack, we need to remove unnecessary CRA files.

## Step 3: VSCode settings

We want to enable format on save on VSCode.

`.vscode/settings.json`

```json
{
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
  },
  "editor.formatOnSave": true,
  "eslint.alwaysShowStatus": true,
  "files.autoSave": "onFocusChange",
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ]
}
```

## Step 4: Make TypeScript more strict

We want to keep type safety as strict as possibble. In order to do that, we update `tsconfig.json` with the settings below.

```jsonc
"noImplicitAny": true,
"noImplicitReturns": true,
```

## Step 5: Installing ESLint

We want to have consistency in our codebase and also want to catch mistakes. So, we need to install ESLint.

```sh
yarn add eslint --dev
```

and then

```sh
yarn run eslint --init
```

Our eslint is giving errors : 
1 - JSX not allowed in files with extension '.tsx' (eslintreact/jsx-filename-extension). To fix this we will add a rule to our eslint file. So open your .eslint file and add part "react/jsx-filename-extension" inside the rules.

2 - "React" must be in scope when using JSX. To fix this we will add a rule to our eslint file. So open your .eslint file and add this line "react/react-in-jsx-scope": "off" inside the rules.

3 - If you open your App.test.js file you will find that eslint is giving us an error that test or expect is not defined . To fix this we need to add "jest": true inside env.

4 - Missing file extension for "./App" (eslintimport/extensions). To fix this we will add a rule to our eslint file. So open your .eslint file and add part "import/extensions" inside the rules and "import/resolver" part inside the settings

`.eslintrc`

```jsonc
{
    "root": true,
    "env": {
        "browser": true,
        "es2021": true,
        "jest": true
    },
    "extends": [
        "plugin:react/recommended",
        "airbnb",
        "plugin:prettier/recommended"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": "latest",
        "sourceType": "module"
    },
    "plugins": [
        "react",
        "@typescript-eslint",
        "prettier"
    ],
    "rules": {
        "react/jsx-filename-extension": [
            1,
            {
                "extensions": [
                    ".js",
                    ".jsx",
                    ".ts",
                    ".tsx"
                ]
            }
        ],
        "import/extensions": [
            "error",
            "ignorePackages",
            {
                "js": "never",
                "mjs": "never",
                "jsx": "never",
                "ts": "never",
                "tsx": "never"
            }
        ],
        "prettier/prettier": [
            "error"
        ]
    },
    "settings": {
        "import/resolver": {
            "node": {
                "extensions": [
                    ".js",
                    ".jsx",
                    ".ts",
                    ".tsx"
                ]
            }
        }
    }
}
```

Create .eslintignore file to provide ignoring for github

`.eslintignore`

```ignore
public
build
react-app-env.d.ts
```


Adjust settings.json for ESLINT

`.vscode/settings.json`

```json
{
  "eslint.validate": ["javascript", "javascriptreact", "typescript", "typescriptreact"]
}
```

We need to update `package.json` for ESLint scripts.

```json
"lint": "yarn run lint:ts",
"lint:ts": "tsc && yarn lint:eslint",
"lint:eslint": "eslint 'src/**/*.{ts,tsx}'",
"format:ts": "prettier --write 'src/**/*.{ts,tsx}' && yarn lint:eslint --fix",
```

## Step 6: Installing Prettier

We want to format our code automatically. So, we need to install Prettier.

```sh
yarn add eslint-config-prettier eslint-plugin-prettier prettier --dev
```

After installing all the above modules it’s time to add some prettier configuration to our .eslintrc.json file. So add this line "plugin:prettier/recommended" inside extends.

`.prettierrc`

```jsonc
{
  "trailingComma": "all",
  "semi": true,
  "singleQuote": false
}
```

`.prettierignore`

```ignore
build
```



Finally, we update `package.json` with related format scripts.

```json
"format:ts": "prettier --write 'src/**/*.{ts,tsx}' && yarn lint:eslint --fix",
"format": "yarn run format:ts",
"format:check": "prettier -c 'src/**/*.{ts,tsx}'"
```


REFERENCES : 
CREATE REACT APP : https://create-react-app.dev/docs/adding-typescript/
ESLINT & PRETTIER : https://medium.com/how-to-react/config-eslint-and-prettier-in-visual-studio-code-for-react-js-development-97bb2236b31a