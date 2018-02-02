# webpack-learning
example from https://laracasts.com/series/webpack-for-everyone  

### 1. Zero Configuration Compilation
`$ npm init -y`  
`$ npm install webpack --save-dev`  
`$ webpack src/main.js dist/bundle.js`  
`$ node_modules/.bin/webpack src/main.js dist/bundle.js --watch`  
### OR
```javascript
{
  "scripts": {
    "build" : "webpack src/main.js dist/bundle.js",
    "watch" : "npm run build -- --watch" 
  }
}
```
`$ npm run watch`

### 2. A Dedicated Configuration File
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

### 3. Modules Are Simply Files
*see code*  
[Notification.js](https://github.com/val-fom/webpack-learning/blob/b1246b08dc664e6fe315118f9172a8ded5d20ba8/src/Notification.js)  
```javascript
function announce (message) {
  alert(message);
}

function log (message) {
  console.log(message);
}

export default {
  announce: announce,
  log: log
}
```
[main.js](https://github.com/val-fom/webpack-learning/blob/b1246b08dc664e6fe315118f9172a8ded5d20ba8/src/main.js)
```javascript
import notification from './Notification';

notification.log('here we go');
notification.announce('here we go as an alert');
```

### 4. Loaders Are Transformers
loaders teaches webpack to read any kid of files   
`$ npm install css-loader --save-dev`
`$ npm install style-loader --save-dev`

```jawascript
module.exports = {
  entry: './src/main.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  }
};
```
styles were injected directly in html

### 5. ES2015 Compilation With Babel
`$ npm install --save-dev babel-loader babel-core`  
`npm install babel-preset-env --save-dev`  
```jawascript
module.exports = {
  entry: './src/main.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      },

      { 
        test: /\.js$/, 
        exclude: /node_modules/, 
        loader: "babel-loader"
      }
    ]
  }
};
```
**.babelrc**
```jawascript
{
  "presets": ["env"]
}
```

### 6. Minification and Environments
we can do this
```jawascript
module.exports = {

  entry: './src/main.js',
  output: {
    //---
  },

  module: {
    //---
  },

  plugins: [
    new webpack.optimize.UglifyJsPlugin()
  ]
};
```
but it is good for production code not for development
so we leave empty plagins arey   
  `plugins: []`
and write if-statement
```javascript
if (process.env.NODE_ENV === 'production') {
  module.exports.plugins.push(
      new webpack.optimize.UglifyJsPlugin()
    )
}
```
to test this we run
`$ NODE_ENV=production node_modules/.bin/webpack`  
now bundle.js id minified

finaly we udate package.json script
```javascript
{ 
 "scripts": {
    "dev": "webpack",
    "production": "NODE_ENV=production webpack",
    "watch": "npm run build -- --watch"
  }
}
```

### 7. Sass Compilation
`$ npm install sass-loader node-sass --save-dev`
```javascript
{
  module: {
    rules: [

      {
        test: /\.s[ac]ss$/,
        use: ['style-loader', 'css-loader', 'sass-loader']
      },

      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      },

      { 
        test: /\.js$/, 
        exclude: /node_modules/, 
        loader: "babel-loader"
      }
    ]
  }
```
now we just paste the styles into html

### 8. Extract CSS to a Dedicated File
`npm install --save-dev extract-text-webpack-plugin`  
```javascript
module.exports = {

  entry: {
    app: [
      './src/main.js',
      './src/main.scss' // now we don't need to call require('./main.scss') in main.js
    ]
  },

  output: {
    path: path.resolve(__dirname, './dist'),
    filename: '[name].js'
  },

  module: {
    rules: [

      {
        test: /\.s[ac]ss$/,
        use: ExtractTextPlugin.extract({
          use: ['css-loader', 'sass-loader'],
          fallback: 'style-loader'
        })
      },

      { 
        test: /\.js$/, 
        exclude: /node_modules/, 
        loader: "babel-loader"
      }

    ]
  },

  plugins: [

    new ExtractTextPlugin("[name].css"),

    new webpack.LoaderOptionsPlugin({ // to decide whether to minimize or not
      minimize: inProduction
    })

  ]
};
```
