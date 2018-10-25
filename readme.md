# React Project Setup
We are going to use following tools
1. Express server to expose react application
2. Webpack to bundle components
3. Babel to transpile
4. Jest unit testing
5. cypress integration testing

# Node Server Setup
1. npm init -y
2. npm i express --save
3. create server.js with Hello world - Refer https://expressjs.com/en/starter/hello-world.**html**
```javascript
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```
4. node server.js 
5. Browse http://localhost:3000 in browser

# Webpack setup
1. npm i webpack, webpack-cli --save-dev
2. Create webpack configuration file
3. We are going to configure webpack to bundle the server component. Following config will do that job for us
```javascript
const backendconfig = {
    target: "node",
    entry: {
        server: './server.js'
    },
    node: {
    __dirname: false
    },
    output: {
        path: `${__dirname}`,
        filename: '[name].bundle.js'
    }
};

module.exports = [backendconfig]
```
4. Edit package.json to configure serve command to bundle and start the server
```javascript
"scripts": {
    "serve": "webpack && node ./server.bundle.js"
  },
```
5. npm run serve - Now you should be able to see the helloworld working again

# React application setup
1. npm i react react-dom --save - install react and reactdom using 
2. create src folder
3. create HTML file index.html
``` html
<html>
    <head>
    </head>
    <body>
        Sample React application
    </body>
</html>
```
4. create index.js to show alert message
5. Lets configure webpack for frontend. We will configure to copy html file alone for now
6. npm i clean-webpack-plugin html-webpack-plugin --save-dev
7. update webpack config with following content
```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');

const frontendconfig = {
    entry: {
        application: './src/index.js'
    },
    output: {
        path: `${__dirname}/frontend`,
        filename: '[name].bundle.js'
    },
    plugins: [
        new CleanWebpackPlugin(`${__dirname}/frontend`),
        new HtmlWebpackPlugin()
    ]
}
```
7. update server.js to expose react content
```javascript
app.use('/frontend', express.static(`${__dirname}/frontend`));
app.get('/', (req, res) => res.sendfile(`${__dirname}/frontend/index.html`))
```
8. Create first react component. Modify the content of the index.js as shown below
```javascript
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(<h1>Sample React Page</h1>, document.getElementById("root"))
```
9. Add a div element into index.html dody
```
<div id="root"></div>
```
## Configure Bable 
1. npm install --save-dev babel-loader @babel/core babel-plugin-transform-runtime @babel/preset-react @babel/preset-env
2. Add bablerc with below content
```javascript
{
    "presets": [
        [
            "@babel/preset-env",
            {
                "useBuiltIns": "entry"
            }
        ],
        "@babel/preset-react"
    ]
}
```
3. update the webpack rule for js to use babel
```javascript
module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: [
                    {
                        loader: 'babel-loader',
                        options: {
                            cacheDirectory: true,
                        },
                    },
                ],
            }
        ]
    },
```
5. npm run serve

# Unit Test
1. npm install --save-dev jest babel-jest regenerator-runtime
2. npm install babel-core@7.0.0-bridge.0 --save-dev
3. Edit package.json file and add script for test as shown below
```javascript
"test": "jest"
```
## Snapshot testing
1. npm i --save-dev react-test-renderer

# Add Navigation 
1.  npm install react-router-dom --save

