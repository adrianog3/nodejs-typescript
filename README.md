# NodeJS Setup - TypeScript + ESLint + Prettier

## How to reproduce this template

The template of this repository can be reproduced as described in the following steps.

### Initial configuration

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

### TypeScript

TypeScript is a highly typed programming language maintained by [Microsoft](https://www.microsoft.com/). All TypeScript code is converted to JavaScript, which consequently can be runs anywhere JavaScript runs: in a [browser](https://en.wikipedia.org/wiki/Web_browser), in [Node.js](https://nodejs.org/en/) or [Deno](https://deno.land/).

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
