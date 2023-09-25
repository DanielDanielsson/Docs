# Eslint format on save

This guide will let you migrate automatic code formatting with Prettier rules defined in a `.prettierrc` or `prettier.config.js` file to automatic formating with eslint. Part 1 of this guide will walk you through the process of migrating your Prettier settings to ESLint. This ensures a seamless transition to using ESLint for code formatting when you enable the 'Format on Save' feature.

However, if you're just interested in configuring VSCode to automatically format your code on save using ESLint, you can skip part 1 and go to part 2. 

## Part 1: Moving Prettier Rules to ESLint

### Install Required Packages:
Before configuring ESLint for formatting, make sure you have [ESLint](https://www.npmjs.com/package/eslint), [Prettier](https://www.npmjs.com/package/prettier), and the Prettier ESLint plugin installed in your project. You can do this by running the following commands in your project's root directory:

```bash
npm install eslint prettier eslint-plugin-prettier eslint-config-prettier --save-dev
```

*NOTE: We need both eslint-plugin-prettier and eslint-config-prettier. The difference between them is that the eslint-config-prettier turns off all rules that are unnecessary or might conflict with Prettier and the eslint-plugin-prettier will allow us to add prettier configuration to eslint rules in `eslintrc.json`.*

### Create or Update .eslintrc.json

In order to migrate your rules from `.prettierrc` you can add them like this: 

```json
{
  "plugins": ["prettier"],
  "extends": ["prettier"],
  "rules": {
    "prettier/prettier": [
      "error",
      {
        "singleQuote": true,
        "endOfLine": "auto",
        "tabWidth": 2,
        "quoteProps": "consistent",
      },
    ],
}
}
```
[Here's a list of all available prettier options](https://prettier.io/docs/en/options)

if you just want prettiers recommended settings, you can add them like this: 
```json
{
  "extends": ["prettier"],
  "plugins": ["prettier"],
  "extends": ["plugin:prettier/recommended"]
}
```
*NOTE: The eslintrc.json file allows us to ommit the "eslint-config-" and "eslint-plugin-" part of the package names, which can make it confusing for when we add "prettier" to both the plugins and extends properties.*

### Remove Prettier Configuration:
If you still have a separate .prettierrc or prettier.config.js file, you can remove it because Prettier rules will now be handled by ESLint.

## Part 2: Configure VSCode Settings for Formatting on Save

### Install Required Extensions:
Ensure you have the [ESLint extension for VSCode](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) installed.

### Configure VSCode Settings:
To enable ESLint to format your code on save, configure your VSCode settings.json settings accordingly:

```json
{
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "dbaeumer.vscode-eslint"
}
```
- "editor.codeActionsOnSave": Enables ESLint to automatically fix issues on save using the ESLint extension.
- "editor.formatOnSave": Enables formatting on save.
- "editor.defaultFormatter": Specifies the default formatter to be the ESLint extension.


*NOTE: If you have a `.vscode` folder with settings in your project, that file will override global settings in VSCode.* 

After making these changes, reload or restart Visual Studio Code for the settings to take effect.

Now, when you save a JavaScript or TypeScript file in your project, ESLint will enforce Prettier formatting rules as defined in your ESLint configuration together with all other eslint rules that you have configured in your project. 🏁
