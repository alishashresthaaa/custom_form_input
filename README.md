# Introduction

<p>This project has a setup of creating and publishing own custom React component library</p>

# Creating components

<p>Because we are creating a library, we are going to create index files for each tier, and export our components from each one to make it as easy as possible for the people using our library to import them.</p>

```
├── src
│ ├── components
| │ ├── Button
| | │ ├── Button.tsx
| | │ └── index.ts
| │ └── index.ts
│ └── index.ts
├── package.json
└── package-lock.json
```

<mark> src/components/Button/Button.tsx </mark>

```
import React from "react";

export interface ButtonProps {
  label: string;
}

const Button = (props: ButtonProps) => {
  return <button>{props.label}</button>;
};

export default Button;

```

## Adding Typescript

```
npx tsc --init

```

> <p>That will create a tsconfig.json file for us in the root of our project that contains all the default configuration options for Typescript.</p>

<p>You may notice depending on your IDE that immediately after initializing you begin to get errors in your project.</p>
<p> There are two reasons for that: the first is that Typescript isn't configuration to understand React by default, and the second is that we haven't defined our method for handling modules yet: so it may not understand how to manage all of our exports.
</p>

<mark>To fix this we are going to add the following values to tsconfig.json:</mark>

```
{
  "compilerOptions": {
    // Default
    "target": "es5",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true,

    // Added
    "jsx": "react",
    "module": "ESNext",
    "declaration": true,
    "declarationDir": "types",
    "sourceMap": true,
    "outDir": "dist",
    "moduleResolution": "node",
    "allowSyntheticDefaultImports": true,
    "emitDeclarationOnly": true,
  }
}

```

# Adding Rollup

> <p> It's very similar to webpack in that it is a tool for bundling individual Javascript modules into a single source that a browser is better able to understand.</p>

<p>Typically webpack is used for bundling applications while rollup is particularly suited for bundling libraries </p>

<p>Rollup uses a plugin ecosystem. By design rollup does not know how to do everything, it relies on plugins installed individually to add the functionality that you need.</p>

```
npm install rollup @rollup/plugin-node-resolve @rollup/plugin-typescript@8.3.3 @rollup/plugin-commonjs rollup-plugin-dts tslib --save-dev
```

- <p>@rollup/plugin-node-resolve - Uses the node resolution algorithm for modules</p>
- <p>@rollup/plugin-typescript@8.3.3  - Teaches rollup how to process Typescript files</p>
- <p>@rollup/plugin-commonjs - Converts commonjs modules to ES6 modules</p>
- <p>rollup-plugin-dts - rollup your .d.ts files</p>
- <p>tslib - peer dependency of rollup-plugin-typescript.</p>

> To configure how rollup is going to bundle our library we need to create a configuration file in the root of our project:

<mark>rollup.config.js</mark>

```
import resolve from "@rollup/plugin-node-resolve";
import commonjs from "@rollup/plugin-commonjs";
import typescript from "@rollup/plugin-typescript";
import dts from "rollup-plugin-dts";
import packageJson from "./package.json" assert { type: "json" };

export default [
  {
    input: "src/index.ts",
    output: [
      {
        file: packageJson.main,
        format: "cjs",
        sourcemap: true,
      },
      {
        file: packageJson.module,
        format: "esm",
        sourcemap: true,
      },
    ],
    plugins: [
      resolve(),
      commonjs(),
      typescript({ tsconfig: "./tsconfig.json" }),
    ],
  },
  {
    input: "dist/esm/types/index.d.ts",
    output: [{ file: "dist/index.d.ts", format: "esm" }],
    plugins: [dts()],
  },
];
```
