# FeM Webpack Fundamentals v4

## Why Webpack? What is it solving?

https://webpack.js.org/concepts/

- Need to understand origins of JS on the web. JS, just a script, top-down execution.
  - Two ways:
  1. Adding a script tag with a src attribute. <script src=""></script>
  2. Write directly into HTML file.
  - Problem with this is that it doesn't scale. Too many scripts trying to load into web browser. Only certain amount of concurrent requests will work.
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
- Modify scripts to debug webpack - npm run debug which runs "debug": "node --inspect --inspect-brk ./node_modules/webpack/bin/webpack.js" in package.json and allows for debugging in webpack
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

- git checkout feature/031-all-module-types -f
- Webpack statically calls what is being used, doesn't show exports that are not being imported
- Webpack core purpose is for managing modules
- Also allows for incremental recompile on styles tied to modules, or other dependencies
- By default, when webpack runs it does a require relative to the local path to webpack.config.js or it attempts to find it

### Webpack Bundle Walkthrough

- Webpack bootstrap - output in main.js - the runtime code at the beginning, then file outputs
- bundle-example-main.js - shows what happens when webpack bundles the code
  - caches modules
  - if exists, exports object from module cache - if module doesn't exist, nothing happens
  - call the module, pass require function to require module outputs
  - webpack require replacing import statements
  - only runtime code, everything else from the build

## Webpack Core Concepts

- Going to add features, loaders, plugins
- Four core concepts - entry, output, loaders, and plugins

### Webpack Entry

- Entry point is not like entry prop on config rather a concept
- Not all dependencies are JS, could be Sass, CSS, etc.
- First JS file to load is your entry to your app, webpack uses this as the starting point
- Defined in the config file
  - webpack.config.js
  - module.exports = {
    entry: "./file_path/filename.ext",
    }
  - webpack traces through imports and look for other dependencies in those files and creates a graph
- Entry - tells webpack what files to load for the browser; compliments the output property

### Output & Loaders

- Now that webpack has entry graph in memory, it needs to create the bundle
- module.exports = {
  entry: "./file_path/filename.ext",
  output: "./dist",
  filename: "./bundle.js",
  }
- There are more configuration options than this one example
- By default, filename is set to main.js and path is dist
- Output tells webpack where and how to distribute bundles (compilations). Works with entry.

#### Loaders & Rules - p94

- Rules tell webpack how to modify files before they are added to dependency graph
  - Ruleset has two basic criteria
  - module: {rules: [{ test: /\.ts$/, use: `ts-loader`}]}
- Loaders are also JS modules (functions) that takes the source file and returns it in a modified state
- test: a regular expression that instructs the compiler which files to run the loader against
- use: an array/string/function that returns loader objects
- enforce: can be pre or post and tells webpack to run this rule before or after other rules
- include: an array or regular expression that instructs the compliler which folder/files to include. Only search paths provided with the include.
- exclude: an array or regular expression that instructs the compiler which folders/files to ignore.
- each property is use case based, won't need to memorize, should be in documentation

### Chaining Loaders - p96

- Anatomy of a loader is a function that takes a source and returns a new source
- Loaders execute right to left
- All sorts of loaders - see p97
- Core concepts - entry, output, loaders
- Tells webpack how to interpret and translate files. Transformed on a per-file basis before adding to the dependency graph.

### Webpack Plugins - p100

- Instance of a JS object that has an applied property on the prototype chain
- Allows you to hook into entire web lifecycle of events
- Objects with an apply property
- Webpack has a variety of plugins out of the box
- How to use plugins
  - require() plugin from node_modules into config.
  - add "new" instance of plugin to plugins key in config object
  - provide traditional info for arguments
- 80% of webpack if made of its own plugin system
- Adds additional functionality to compilations (optimized bundled modules). More powerful w/ more access to CompilerAPI. Does everything else you would ever want to do in webpack.

### Webpack Config

- Exercise with mode and output w/ bundle.js

### Adding Webpack Plugins

- Like to have html-webpack-plugin
- Added webpack for more terminal info, it's out of the box
- Added build-utils folder - where the build happens, includes supplemental or partial configurations end up

### Setting Up Local Dev Server

- Install webpack-dev-server
- Provides development environment on localhost with dynamic updates
- Web server based on express, all it's doing instead of creating bundle to dist folder, generates a bundle in memory and serves to express and does websocket connection and reloads

### Splitting Environment Config Files

- const modeConfig = env => require(`./build-utils/webpack{env}`)(env);
- Based on what we require, it's going to look for production or development
- Leveraging env.mode, passing it in
- module.exports = ({ mode, presets } = { mode: "production", presets: [] })
- Defaulting presets as fall back in case something goes wrong
- Sets up webpack.production.js and webpack-development.js with presets

## Using Plugins

- Any time you change config, you have to rerun the server

### Using CSS with Webpack

- Traditional way is to serve in dist folder and referencing in style tag
- If it is affecting our code, we would want to have it tied in with our build, change without reloading
- Updated dev to include footer.css
- Multiple CSS files will load within one file - main.css

* inline - hard coding styles to the element tags

### Hot Module Replacement with CSS

- Loaders are really useful for helping support unique webpack feature called hot module replacement
- Use --hot in script in package.json
- Capability to patch changes incrementally without reloading the browser
- Good for something that has a lot of complex state and a cool dev experience
- Used MiniCssExtractPlugin - https://webpack.js.org/plugins/mini-css-extract-plugin/

### File Loader & URL Loader

- Common loaders for dev tool capabilities
  - file-loader
  - url-loader

### Loading Images with Javascript

- Loader allows for importing image

### Limit Filesize Option in URL Loader

- url-loader has a limit option
- Can pass options to use objects

### Implementing Presets

- There is going to be more than dev and prod and you will want to analyze a feature but you don't want it shipped in dev or prod
- Can be called add-ons
- Can add preset to package.json script
- One preset that is valuable to add is bundle-analyzer

### Bundle Analyzer Preset

- webpack-bundle-analyzer - https://www.npmjs.com/package/webpack-bundle-analyzer
- Webpack by default emits a stats object. It is info you see anytime a build happens.
- Can set to static w/ port forward to analyze each build. code-gov-front-end uses but not sure where it outputs.

### Compression Plugin

- compression-webpack-plugin - https://webpack.js.org/plugins/compression-webpack-plugin/
- Takes any assets and compresses them with Gzip (https://www.gnu.org/software/gzip/)

### Source Maps

- Devtool - responsible for creating source maps
