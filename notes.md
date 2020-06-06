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

## Exercise files
- Included in this repo but need to clone to follow along because of branch switching.
- GH repo w/ branches - https://github.com/TheLarkInn/webpack-workshop-2018.git
- Switch to Node v8.0.0 (https://itnext.io/nvm-the-easiest-way-to-switch-node-js-environments-on-your-machine-in-a-flash-17babb7d5f1b)
    - nvm ls - check installed versions of Node
    - install nvm 8.0.0 - installed Node v8
    - nvm use 10.17.0 - to switch (don't need on install)

- git checkout feature/01-fem-first-script
    - package.json -> "scripts": { 
        "key": "value"
        "webpack": "webpack"
        } 
    - terminal -> npm run <name-of-that-script> (e.g. npm run webpack)
    - webpack looks for an entry property and defaults to src/index.js
  