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

# React Concepts
## Component
### State Less component
Method that returns HTML element is knowns as stateless component
```Javscript
import React from 'react';


export function Stateless(props) {
    return (
        <div>
            <table>
                <tr>
                    <td>
                        {props.name}
                    </td>
                </tr>
                <tr>
                    <td>
                        {props.age}
                    </td>
                </tr>
            </table>
            <br></br>
        </div>
    );
}
```
### State full component
Class components are called as state full component. This can store local state and manipulate that locally
```Javascript
import React, { Component } from 'react'
export default class StateFull extends Component {
    constructor(props) {
        super(props);
        this.state = { clicks: 0 };
    }
    render() {
        return (
            <div onClick={() => { this.setState({ clicks: this.state.clicks + 1 }); }}> Hello {this.props.name}, you Clicked {this.state.clicks} </div>
        );
    }
}
```
## Data
Data can be passed to a components only using props. A component can store it's local state, if it is class component

### Props
Passing props to a component instance

```Javascript
<Stateless name="senthil" age="35"></Stateless>
```

Props value can be accessed inside the component like this
```Javascript
this.props.name -- State full component
props.name -- state less component
```


### local state
Class component can define it's state. Following code shows how the class component initialize local state
```Javascript
constructor(props) {
        super(props);
        this.state = { clicks: 0 };
    }
```

Local state can be accessed using following code
```Javascript
this.state.clicks
```

Local state can be updated like below
```Javascript
this.setState({ clicks: this.state.clicks + 1 })
```

## Routing
Client side routing are provided using react-router component. To do a client side routing we need 3 components
1. Router - Based on the path render configured components for that path.
2. Link - Provides a navigation to navigate to particular path
3. Route - Associate component for a particular path

Based on the type of application (Browser/Native) we need to select appropriate router
For example for Browser based application BrowserRouter is provided by react-router-dom
Following code shows how to add a router, route and link

```Javascript
import React, { Component } from 'react'
import { Stateless } from './Stateless';
import StateFull from './SampleState';
import { BrowserRouter, Link } from 'react-router-dom'
import { Route } from 'react-router'


export default class App extends Component {
    render() {
        return (
            <BrowserRouter>
                <div>
                    <table>
                        <tr>
                            <td><Link to="/stateless">stateless</Link></td>
                            <td> <Link to="/statefull">statefull</Link></td>
                        </tr>
                    </table>
                    <Route path="/stateless" component={() => <Stateless name="senthil" age="35"></Stateless>}></Route>
                    <Route path="/statefull" component={() => <StateFull name="senthil"></StateFull>}></Route>
                </div>
            </BrowserRouter>
        );
    }
}
```
