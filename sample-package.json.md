# Sample package.json scripts for webpack

"scripts": {
    "webpack": "webpack",
    "debug": "node --inspect --inspect-brk ./node_modules/webpack/bin/webpack.js",
    "prod": "npm run webpack -- --mode production",
    "dev": "npm run webpack -- --mode development",
    "prod:debug": "npm run debug -- --mode production",
    "dev:debug": "npm run debug -- --mode development",
}