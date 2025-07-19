# Setup Tailwind-Css in Remix Shpoify App

This is a step by step guide to setup tailwind-css in any Remix Shopify App.
If you are bored of being forced to use Shopify Polaris and it's booring component's and you are missing the customization in you UI as you like the just follow the steps below to setup tailwind-css v3 in you app.

I only managed to setup tailwind-css v3, v4 is not working if you find a way please let me know as well

## Installation

Setup a fresh Remix Shopify App. Fresh because it wont be conflicting with other stuff you can try it on existing app as well.

```bash
shopify app init
```

## Tailwind v3 Install

Wait for App to be setup then follow the below steps. Install tailwind-css v3.
*Note*:- If you are going to use Docker to deploy the app then you need to install tailwind as dependency not as devDependency so remove the -D tag from below command.

```bash
npm install -D tailwindcss@3 postcss autoprefixer

npx tailwindcss init -p
```

This will install tailwind and create 2 files in your project root.

> tailwind.config.js
>
> postcss.config.js

## File Changes

1. In `vite.config.js` file, find ->

```bash
optimizeDeps: {
    include: ["@shopify/app-bridge-react", "@shopify/polaris"],
  },
```
and remove @shopify/polaris from the *include* array ->
```bash
optimizeDeps: {
    include: [ "@shopify/app-bridge-react" ],
  },
```

2. In `postcss.config.js` leave it default it should look like this ->

```bash
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```
3. In `tailwind.config.js` change the content array with ->
```bash
  content: [
    // Add paths to your .tsx, .jsx, .ts, and .js files
    "./app/**/*.{ts,tsx,js,jsx}",
    "./app/components/**/*.{ts,tsx,js,jsx}",
    "./app/routes/**/*.{ts,tsx,js,jsx}",
    // Add other routes according to the structure of your project
  ],
```
4. Make a new folder `styles` in `/app` directory parallel to `routes` folder.

Inside `styles` folder make a file `tailwind.css` and past the below line into it.
```bash
/* app/styles/tailwind.css */

@tailwind base;
@tailwind components;
@tailwind utilities;
```
5. Make a new file `root.css` in same `/app` directory and past the lines given below.
```bash
/* app/root.css */

@import "./styles/tailwind.css";

/* You can add additional global styles here */
```
6. In same `/app` directory you will find a file name [**root.js** || **root.jsx** || **root.ts** || **root.tsx**] according to your project setup language.

In this file add the following lines under the `"@remix-run/react"` imports.
```bash
import stylesUrl from "./root.css?url"; // Import as URL

// change here if you are using ts
export const links = () => [
  { rel: "stylesheet", href: stylesUrl },
];
```
7. Now you can run the app and test the tailwind-css by using it in `app._index.jsx` file.
```bash
export default function Index() {
  const fetcher = useFetcher();
  const shopify = useAppBridge();

  return (
    <Page>
      <TitleBar title="Remix app template">
        <button variant="primary">
          Generate a product
        </button>
      </TitleBar>
      {/* Testing if tailwind is working */}
      <div className="w-full h-[100vh] bg-black text-white">
        Hello from Tailwind Css
        is this working
      </div>
    </Page>
  );
}
```
