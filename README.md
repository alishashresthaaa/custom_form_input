### Introduction

This project has a setup of creating and publishing own custom React component library

### Creating components

Because we are creating a library, we are going to create index files for each tier, and export our components from each one to make it as easy as possible for the people using our library to import them.

├── src
│ ├── components
| │ ├── Button
| | │ ├── Button.tsx
| | │ └── index.ts
| │ └── index.ts
│ └── index.ts
├── package.json
└── package-lock.json

==src/components/Button/Button.tsx==

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

### Adding Typescript

```
npx tsc --init

```

That will create a tsconfig.json file for us in the root of our project that contains all the default configuration options for Typescript.
You may notice depending on your IDE that immediately after initializing you begin to get errors in your project. There are two reasons for that: the first is that Typescript isn't configuration to understand React by default, and the second is that we haven't defined our method for handling modules yet: so it may not understand how to manage all of our exports.

==To fix this we are going to add the following values to tsconfig.json:==

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
