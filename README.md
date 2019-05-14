# Initial project setup with custom webpack and babel configuration

Open up the command line and create a project directory

## Installation required
1 - Initialize a package.json by running:

``` bash
npm init -y
```
2 - Now install react 

``` bash
npm install react react-dom
```

3 - Install Webpack, webpack-dev-server, webpack cli

``` bash
npm install --save-dev webpack webpack-dev-server webpack-cli
```

__This will install the following__

__webpack module__ — which include all core webpack functionality

__webpack-dev-server__ — this development server automatically rerun webpack when our file is changed

__webpack-cli__ — enable running webpack from the command line


4 - Add the following script to package.json

```
"scripts":
{
 "dev": "webpack-dev-server --mode development",
 "prod": "webpack --mode production"
},
```

5 - Create HTML file in the root folder

```
<!DOCTYPE html>
<html>
 <head>
 <title>Configuration Setup</title>
 </head>
 <body>
 <div id="root"></div>
 <script src="./dist/bundle.js"></script>
 </body>
</html>
```

6 - Create a new directory named **src** and inside it, create a new **index.js** file
```
mkdir src && cd src && touch index.js
```
7 - Write a React component into the file:

```
import React from "react";
import ReactDOM from "react-dom";
class App extends React.Component {
  render() {
    return <h1>React Configuration</h1>;
  }
}
ReactDOM.render(<App />, document.getElementById("root"));
```

### Configuring Babel 7 
This will basically converts all the ES6 to ES5 syntax.

__Let’s install Babel into our project__

```
npm install --save-dev @babel/core @babel/preset-env \
@babel/preset-react babel-loader
```

**@babel/core** is the main dependency that includes babel transform script.

**@babel/preset-env** is the default Babel preset used to transform ES6+ into valid ES5 code. Optionally configures browser polyfills automatically.

**@babel/preset-react** is used for transforming JSX and React class syntax into valid JavaScript code.

**babel-loader** is a webpack loader that hooks Babel into webpack. We will run Babel from webpack with this package.

#### Let's write a webpack.config.js
```
const HtmlWebPackPlugin = require("html-webpack-plugin");
const path = require("path");
var BUILD_DIR = path.resolve(__dirname, "build/");
var SOURCE_DIR = path.resolve(__dirname, "src/");
module.exports = {
  context: __dirname,
  entry: SOURCE_DIR + "/index.jsx",
  output: {
    path: BUILD_DIR,
    filename: "bundle.js",
    publicPath: "/"
  },
  performance: {
    maxEntrypointSize: 512000,
    maxAssetSize: 512000
  },
  devServer: {
    historyApiFallback: true
  },
  resolve: {
    extensions: [".js", ".jsx"]
  },
  module: {
    rules: [
      {
        test: /\.js|jsx$/,
        include: SOURCE_DIR,
        exclude: /node_modules/,
        use: "babel-loader"
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: path.resolve(__dirname, "./index.html"),
      filename: "index.html"
    })
  ]
};

```

##### Finally create a new .babelrc file
```
touch .babelrc
```

Copy the following content:

```
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}
```
##Optional

##### Adding Prettier

```
npm install --save-dev --save-exact prettier
```

Now we need to write the .prettierrc configuration file:

```
{
 "semi": true,
 "singleQuote": true,
 "trailingComma": "es5"
}
```

Add script to our package.json file:

```
"scripts": {
 "format": "prettier --write \"src/**/*.js\""
},
```

__you can run following command__

```
 npm run format
```

VSCode for development, you can install the Prettier extension and run it every time you save your changes by adding this setting:

```
"editor.formatOnSave": true
```


#### Setting up ESLint

```
Setting up ESLint
```

**Modifiy the config**

```
  module: {
    rules: [{
      test: /\.js?$/,
      exclude: /node_modules/,
      use: ['babel-loader', 'eslint-loader'] // include eslint-loader
    }]
  },
  ```