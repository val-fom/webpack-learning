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
