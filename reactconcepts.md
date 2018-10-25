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
