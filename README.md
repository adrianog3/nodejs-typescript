# NodeJS Setup - TypeScript + ESLint + Prettier

## How to reproduce this template

The template of this repository can be reproduced as described in the following steps.

## Initial configuration

Execute the command bellow to install [Yarn](https://yarnpkg.com/), the alternative package manager over [npm](https://www.npmjs.com/).

```
npm install -g yarn
```

Run the command below and yarn will guide you through creating the `package.json` file.

```bash
yarn init -y
```

Add property `private` as `true` in `package.json` file. This is a way to prevent accidental publication of private repositories.

> To simplify others properties have been omitted here and in the rest of the code snippets in this guide.

```json
{
  "private": true
}
```

## TypeScript

[TypeScript](https://www.typescriptlang.org/) is a highly typed programming language maintained by [Microsoft](https://www.microsoft.com/). All TypeScript code is converted to JavaScript, which consequently can be runs anywhere JavaScript runs: in a [browser](https://en.wikipedia.org/wiki/Web_browser), in [Node.js](https://nodejs.org/en/) or [Deno](https://deno.land/).

```bash
yarn add typescript -D
```

After adding the dev dependency, create a `tsconfig.json` file under the root of the project.

```json
{
  "compilerOptions": {
    "target": "es2021",
    "module": "commonjs",
    "outDir": "build",
    "rootDir": "src",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true
  },
  "include": ["src"],
  "exclude": ["node_modules"]
}
```

This file above configures the behavior of the compiler.

Now let's understand some properties:

- [`es2021`](https://262.ecma-international.org/12.0/) features will be used when generating the distributable version
- use the [`commonjs`](https://nodejs.org/docs/latest/api/modules.html) module system.
- generate JavaScript files inside `/build` folder
- include all files inside `/src` folder
- exclude all files inside `/node_modules` folder
- the other properties are recommended by typescript and you can check the meaning here: https://www.typescriptlang.org/tsconfig

The next step is install `ts-node-dev` dependency. It's a tool that compiles your projects with typescript and restarts the project when any file is modified.

```bash
yarn add ts-node-dev -D
```

Add `scripts` property in your `package.json` file.

```json
{
  "scripts": {
    "build": "tsc",
    "dev": "ts-node-dev src/index.ts"
  }
}
```

Now create the main file `src/index.ts` and fill it with the following content.

```typescript
console.log("Hello World!");
```

This configuration allows you to run the command `yarn dev` to execute application in development mode.

The `build` property in `scripts` run the typescript compiler (tsc) and generate a distributable version inside `/build` folder. To check the behavior run the `yarn build` command.

## ESLint

[ESLint](https://eslint.org/) is a code analysis tool to identify problems and bad practices in JavaScript and TypeScript code.

To install this package, run the following command:

```bash
npx eslint --init
```

**How would you like to use ESLint?**

> To check syntax, find problems, and enforce code style

**What type of modules does your project use?**

> JavaScript modules (import/export)

**Which framework does your project use?**

> None of these

**Does your project use TypeScript?**

> Yes

**Where does your code run?**

> Node

**How would you like to define a style for your project?**

> Use a popular style guide

**Which style guide do you want to follow?**

> Airbnb: https://github.com/airbnb/javascript

**What format do you want your config file to be in?**

> JSON

**Would you like to install them now with npm?**

> Yes

As we are using `yarn` remove the `package-lock.json` file that was created by the previous command and run `yarn install` to update the package references.

After completing the installation, the `.eslintrc.json` file will be created inside root folder

```json
{
  "env": {
    "es2021": true,
    "node": true
  },
  "extends": ["airbnb-base"],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint"],
  "rules": {}
}
```

and some dev dependencies will be added in `package.json` file

```diff
{
  "devDependencies": {
+   "@typescript-eslint/eslint-plugin": "^5.10.2",
+   "@typescript-eslint/parser": "^5.10.2",
+   "eslint": "^8.8.0",
+   "eslint-config-airbnb-base": "^15.0.0",
+   "eslint-plugin-import": "^2.25.4",
    "ts-node-dev": "^1.1.8",
    "typescript": "^4.5.5"
  }
}
```

If you are using [VSCode](https://code.visualstudio.com/) is necessary install the [ESLint Plugin](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint).

Now let's add a script to the `package.json` file to see eslint in action.

```diff
{
  "scripts": {
    "build": "tsc",
    "dev": "ts-node-dev src/index.ts",
+   "lint": "eslint src --ext .ts"
  }
}
```

Using this command any file with `.ts` extension inside `src` folder will be analyzed by the eslint plugin. So when running:

```bash
yarn lint
```

the result should be similar to this:

<img align="left" alt="eslint output" style="max-width: 680px" src="assets/lint-output.png" />
