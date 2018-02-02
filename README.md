# webpack-learning
example from https://laracasts.com/series/webpack-for-everyone  
### Zero Configuration Compilation
`$ npm init -y`  
`$ npm install webpack --save-dev`  
`$ webpack src/main.js dist/bundle.js`  
`$ node_modules/.bin/webpack src/main.js dist/bundle.js --watch`  
#### OR
```javascript
{
  "scripts": {
    "build" : "webpack src/main.js dist/bundle.js",
    "watch" : "npm run build -- --watch" 
  }
}
```
`$ npm run watch`
### A Dedicated Configuration File
```javascript
var webpack = require('webpack');
var path = require('path');

module.exports = {
    entry: './src/main.js',
    output: {
        path: path.resolve(__dirname, './dist'),
        filename: 'bundle.js'
    }
};
```
`$ npm run watch`
### Modules Are Simply Files
*see code*
