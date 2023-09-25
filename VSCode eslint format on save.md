# Eslint format on save

## Part 1: Moving Prettier Rules to ESLint (Using Prettier ESLint Plugin)

Install Required Packages:
Before configuring ESLint for formatting, make sure you have ESLint, Prettier, and the Prettier ESLint plugin installed in your project. You can do this by running the following commands in your project's root directory:

```bash
npm install eslint prettier eslint-plugin-prettier eslint-config-prettier --save-dev
```

Create or Update .eslintrc.json:
In your project's root directory, create or update your ESLint configuration file (.eslintrc.json). Add the following configurations to it:

```json
{
  "plugins": ["prettier"],
  "extends": ["eslint:recommended", "plugin:prettier/recommended"]
}
```

Or, if you already have a `.prettierrc` file with rules you want to keep, you can add it lite this: 

```js
rules: {
'prettier/prettier': [
      'error',
      {
        singleQuote: true,
        endOfLine: 'auto',
        tabWidth: 2,
        quoteProps: 'consistent',
      },
    ],
}
```

This configuration extends the recommended ESLint rules and integrates the Prettier ESLint plugin.

Remove Prettier Configuration:
If you have a separate .prettierrc or prettier.config.js file, you can remove it because Prettier rules will now be handled by ESLint.
