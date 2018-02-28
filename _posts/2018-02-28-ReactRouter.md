---
layout: post 
title: React Router Basics
category: normal
---

React Router handles the routing of urls and views in our React projects. It has three packages: "react-router" the core package, "react-router-dom" for web apps and "react-router-native" for mobile apps. Over here we're only interested in building a browser based app, so we will be using "react-router-dom".

To start, do:
```shell
yarn add react-router-dom 
# or
npm install --save react-router-dom
```


### Router
Within react-router-dom, there are two routers that we can use:

[`BrowserRouter`](https://reacttraining.com/react-router/web/api/BrowserRouter)  uses the HTML5 history API to keep track of our router history, e.g.  https://huiwenteo.com/posts.

[`HashRouter`](https://reacttraining.com/react-router/web/api/HashRouter)  uses the hash portion of the url: window.location.hash, e.g. https://huiwenteo.com/#/posts.

We'll be using BrowserRouter in our examples below.



### Basic example 

To get started, simply wrap our <Main /> component within BrowserRouter. Main.js is our starting point of our React project. Note that a router component can only have one child component. 
```javascript
// index.js 
import React from "react";
import ReactDOM from "react-dom";

// importing BrowserRouter for ReactRouter to work 
import { BrowserRouter } from "react-router-dom";

// Main is our starting point of the React project
import Main from "./Main";

// A router component can only have one child component, in our case its <Main />.
ReactDOM.render(
  <BrowserRouter>
    <Main />
  </BrowserRouter>,
  document.getElementById("root")
);
```

We then specify a few routes to get started. We will be going through the Switch and Route components below. 

```javascript
// Main.js
import React from "react";
import { Switch, Route } from "react-router-dom";
import Home from "./Home";
import Login from "./Login";

const Main = () => {
  return (
    <Switch>
      <Route exact path="/" component={Home} />
      <Route path="/login" component={Login} />
      <Route path='/about' render={(props) => <About {...props} extra={someVariable} />} />
      // Render this when no route matches the ones we have defined
      <Route component={Render404Component} /> 
     </Switch>
  );
};
```


### Route component and Route matching 
The `<Route>` component has 3 props: `path, render and component`. 

`path`: This specifies the portion of the url that we want to match against the current location

`component`: We use this when we want to render an existing component. 

`render`: We use this only when we want to pass variables to the component we want to render. It takes in a function and expects the function to return a component. 

When a `<Route>` path prop matches with the url, the component we specified will be rendered. 

Note: If we want a route to be rendered only if the paths are exactly the same, we use the exact prop.



### Switch component and Route matching 
In the above Main.js example, the `<Switch>` component will iterate over its children and render the first `<Route>` path prop that matches the current url. The Switch component is not needed but is useful if we have multiple routes that have the same pathname, or if we want to render a 404 component when a user enters a url that's not there. 

If we do not wrap our routes inside a `<Switch>` component, the components that matches the current path will be rendered, regardless of how many components are matched. For example, if we have 


```javascript
<Route exact path="/" component={Home}/>
<Route path="/login" component={login}/>
<Route path="/:id" render = {()=> (<p> I want this text to show up for all routes </p>)}/>
```
 and our current route is `/login`, the last route with path="/:id" will be rendered as well. Example taken from [here](https://www.sitepoint.com/react-router-v4-complete-guide/#switchcomponent). 


### Link and Redirect components 
Each BrowserRouter component has a history object that keeps track of the current location and the previous locations in a stack. Both link and redirect makes use of this history object. 

`Link`: The Link component uses the `history.push('/')` method. A `<a>` will be rendered in the HTML.
```javascript
<Link to="/" />
```

`Redirect`: The Redirect component calls the `history.replace('/')` method and forces navigation using its `to` prop.
```javascript
<Redirect to='/' />
```


### Custom Routes (Authentication)

Now we know the basics of the React Router, we can move on to authentication! Basically, creating a private route that checks whether a user is authenticated before rendering the desired page. We first specify the private route and pass the auth.isAuthenticated value to it.


```javascript
// Main.js
import React from "react";
import { Switch, Route } from "react-router-dom";
import Home from "./Home";
import { auth, Login } from "./login";  // import auth from login

const Main = () => {
  return (
    <Switch>
      <Route exact path="/" component={Home} />
      <Route path="/login" component={Login} />
      <Route path='/about' render={(props) => <About {...props} extra={someVariable} />} />
      // Render this when no route matches the ones we have defined
      <Route component={Render404Component} />
      // We specify a private route that only renders if user is logged in 
      <PrivateRoute auth={auth.isAuthenticated} path="/admin" component={AdminDashboard} />
     </Switch>
  );
};
```

```javascript
// PrivateRoute.js 
const PrivateRoute = ({component: Component, auth, ...rest}) => {
  return (
    <Route
      {...rest}
      render={(props) => auth === true
        // if auth is true
        ? <Component {...props} />
        // else
        : <Redirect to={ {pathname: '/login', state: {from: props.location}} } />} />
  )
}
```

```javascript
// login.js
...
// fake auth function 
export const auth = {
	isAuthenticated: false,
	authenticate(cb) {
		this.isAuthenticated = true
	}
}
```

We export auth in login.js to be able to use it in Main.js, which will provide information of a user's authentication in the private route component. The PrivateRoute component specifies that the AdminDashboard component is rendered only if user is logged in, else it will redirect users to the login page. 


That's it for react router basics, this post was a condensed version of the sitepoint react router guide. There are other demos in it that is really useful, such as using the history's match object for nesting routes. If you're interested to learn more, do visit the site to check it out! 


### Resources
- [react router guide from sitepoint](https://www.sitepoint.com/react-router-v4-complete-guide/)
- [react router web training docs](https://reacttraining.com/react-router/web/guides/philosophy)
