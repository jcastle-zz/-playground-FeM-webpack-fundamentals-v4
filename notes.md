# FeM Webpack Fundamentals v4

## Why Webpack? What is it solving?
- Need to understand origins of JS on the web. JS, just a script, top-down execution.
    - Two ways:
    1. Adding a script tag with a src attribute. <script src=""></script>
    2. Write directly into HTML file.
    * Problem with this is that it doesn't scale. Too many scripts trying to load into web browser. Only certain amount of concurrent requests will work.
- Having on global.js w/ 10k lines does not work either because of scope, size, readability, fragility, and monolith files.
- Previous toolings for concatenating encapsulated (IIFE) files. Previous tools include Make, Grunt, Gulp, Broccoli, etc.
- Lots of IIFEs are slow which is why concatenating doesn't work well.

### History of Modules
- Modules based on NodeJS. If you want to build a canonical website, you will use Node.
(When referring to programming, canonical means conforming to well-established patterns or rules. The term is typically used to describe whether or not a programming interface follows the already established standard. - https://www.webopedia.com/TERM/C/canonical.html)
- NodeJS is foundational for working with packages, working with scripts, working with a server.
- NPM + Node + Modules - led to mass distribution and explosion of JS.
- NPM created as package registry to share CommonJS Node modules across an ecosystem or registry.
- Bundlers/linkers - main premise to write CommonJS modules to get bundled to strip statements and execute on the web. Different approaches are loaders. JS to fetch various module formats.
- CommonJS - led to problems with abusing require and generally led to inefficiencies.
    - No static async.
    - No lazy loading (all bundles up front).
    - Too dynamic of module format for optimized code.
    - Not everyone shipping module JS.
- Other solutions besides CommonJS included AMD and ESM.

### Webpack
- https://github.com/webpack/webpack
- Webpack is a module bundler. Let's you write any module format and allows them to work in the browser.
- Webpack supports code splitting or static async bundling. You can create separate lazy loaded bundles at building time allowing for extra optimizations.

### Configuring Webpack
- Three ways to use Webpack:
    1. Config (it's a module too) - describes to Webpack what should be done with the code.
    2. Webpack CLI - can use it this way without touching a config file.
    3. Node API - for a Webpack wrapper. 

### Q&A
- Module system allows for maintaining and scaling w/ bundlers.
- No other syntax as unique as ESM (EcmaScript Modules).
- In web, need to care about how much code compiling and shipping.

## Webpack from Scratch
- Included in this repo but need to clone to follow along because of branch switching.
- GH repo w/ branches - https://github.com/TheLarkInn/webpack-workshop-2018.git
- Switch to Node v8.0.0 (https://itnext.io/nvm-the-easiest-way-to-switch-node-js-environments-on-your-machine-in-a-flash-17babb7d5f1b)
    - nvm ls - check installed versions of Node
    - install nvm 8.0.0 - installed Node v8
    - nvm use 10.17.0 - to switch (don't need on install)

### Adding NPM Scripts for ENV Builds
- git checkout feature/01-fem-first-script
    - package.json -> "scripts": { 
        "key": "value"
        "webpack": "webpack",
        "dev": "npm run webpack -- --mode development",
        "prod": "npm run webpack -- --mode production"
        } 
    - terminal -> npm run <name-of-that-script> (e.g. npm run webpack)
    - webpack looks for an entry property and defaults to src/index.js
        - no config file needed, run webpack and default to src and index.js
    - NPM has capability to compose scripts
        - Have property in webpack to designate dev or prod or other

### Setting up Debugging
- git checkout feature/03-fem-debug-script --force
    - "debugthis": "node --inspect --inspect-brk ./src/index.js"
    - ./src/index.js is to the source file, can be anything you wish to debug
    - run debug script and results in URL, go to Chrome and type chrome://inspect
    - click link "open dedicated dev tools for Node"
    - open console and type process.env or global
- Modify scripts to debug webpack - npm run debug which runs "debug": "node --inspect       --inspect-brk ./node_modules/webpack/bin/webpack.js" in package.json and allows for debugging in webpack
    - helpful for writing custom loader or plugin and good for debugging
    - command p - provides file picker while in webpack
    - async is hard, debugger provides valuable context
- Scripts - focus on composition and separation of concerns, most issues because everything shoved into one script or build file

### Coding Your First Module
- create ./src/nav.js and export default "nav";
- in ./src/index.js import nav from "./nav"; and console.log(nav);
- npm run prod to run files in webpack and webpack adds a dist folder w/ main.js
- run node ./dist/main.js and returns nav - used two modules in index.js and nav.js with functionality executed in one module located in main.js

### Adding Watch Mode
- Webpack has a watching mode which means we don't have to run npm run prod many times
- Added with a --watch flag on the end of the dev script
- As changes are made, webpack will continually update the main.js file

### ES Module Syntax
- With watch, we can continue to add modules
- Using export and import on js pages and using watch for results

### Common JS Export
- React uses CommonJS and will need to know how to export accordingly
- Format is similar to ES Module Syntax
- In Webpack, cannot use CommonJS and ES syntax together but do have interop to work with both for dependencies and live bindings

### CommonsJS Named Exports
- Should not use CommonJS if possible
- Babel and Typescript default to CommonJS and pass to webpack
- Const at the top of file, Exports at bottom of file
- Don't have to import every export, can choose the ones you want, and only pull in what you are using

### Tree Shaking



