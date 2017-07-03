Create React App doesn't handle backend logic or databases; it just creates a frontend build pipeline, so you can use it with any backend you want. It uses webpack, Babel and ESLint under the hood, but configures them for you.

While React can be used without a build pipeline, we recommend setting it up so you can be more productive. A modern build pipeline typically consists of:

A package manager, such as Yarn or npm. It lets you take advantage of a vast ecosystem of third-party packages, and easily install or update them.
A bundler, such as webpack or Browserify. It lets you write modular code and bundle it together into small packages to optimize load time.
A compiler such as Babel. It lets you write modern JavaScript code that still works in older browsers.
<hr>

# JSX
- Since JSX is closer to JS, React DOM uses camelCase naming convention.
- JSX prevents Injection Attacks 
- Can embed any JS expression in JSX by wrapping it in curly braces. E.g: 
```
function formatName(user) {
	return user.firstName + ' ' + user.lastName;
}

const user = {
	firstName: 'Harper',
	lastName: 'Perex'
};

const element = (
	<h1>
	Hello, {formatName(user)}!
	</h1>
);

ReactDOM.render(
	element,
	document.getElementById('root')
};
```

After compilation, JSX expressions become regular JS objects => which means, we can use JSX inside if statements & for loops, assign to var etc. 

- Specifying Attributes w JSX: use either quotes for string literals || curly braces to embed JS exp. Don't use both in same attr! E.g:
```
const element = <div tabIndex="0"></div>;
OR
const element = <img src={user.avatarUrl}></img>;
```

- Specifying children:
```
const element = (
	<div>
		<h1>Hello!</h1>
		<h2>Good to see you here.</h2>
	</div>
);
```


- JSX represents objects. These are the same: 
```
const element = {
	<h1 className="greeting">
		Hello world!
	</h1>
};

const element = React.createElement(
	'h1',
	{className: 'greeting'},
	'Hello world!'
);
```

React.createElement() creates an obj similar to:
```
const element = {
	type: 'h1',
	props: {
		className: 'greeting',
		children: 'Hello world'
	}
};
```

These objs are called React elements. React reads these objects & uses them to construct the DOM & keep it up to date.
<hr>

# Rendering an Element into the DOM
Let's say there's a div in our HTML file like this: 
```
<div id="root"></div>
```
Apps built with React usually have a single root DOM node. To render a React element into a root DOM node, we pass both to ReactDOM.render():
```
const element = <h1>Hello, world</h1>;
ReactDOM.render(
	element, 
	document.getElementById('root')
);
```
<hr>
# Immutable React Elements: Updating the rendered element
 Once we create an element, we can't change its children or attributes. Elements are immutable. 
```
// It calls ReactDOM.render() every second from a setInterval() callback. React only updates what's necessary. 
function tick() {
	const element = (
		<div>
			<h1>Hello, world!</h1>
			<h2>It is {new Date().toLocaleTimeString()}.</h2>
		</div>
	);
	ReactDOM.render(
	    element, 
		document.getElementById('root')
	);
}

setInterval(tick, 1000);
```
<hr>

## Components & Props
Components lets us split the UI into independent, reusable pieces & think bout each piece in isolation === similar to JS functions, accept arbitrary inputs called props & return React Elements describig what should appear on the screen. 

# Function & Class Components 
Simplest way to define a comp is to write JS func. Its a valid R Comp cause it accepts a single 'props' obj arg with data & return React element.
```
function Welcome(props) {
	return <h1> Hello, { props.name } </h1>;
}
```
Or, we can use ES6 class to define a comp:
```
class Welcome extends React.Component {
	render() {
		return <h1> Hello, { props.name } </h1>;
	}
}
```

# Rendering a component 
Elements can also represent components. When R sees an element representing a user-defined comp, it passes JSX attr to this comp as a single obj => "props".
```
// This code renders "Hello, Sara" 
function Welcome(props) {
	return <h1> Hello, { props.name } </h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
    element, 
	document.getElementById('root')
);
```
Steps for the above:
1. We call ReactDOM.render() with the <Welcome name="Sara" /> element.
2. React calls the Welcome component with {name: 'Sara'} as the props.
3. Our Welcome component returns a <h1>Hello, Sara</h1> element as the result.
4. React DOM efficiently updates the DOM to match <h1>Hello, Sara</h1>.
NOTE: Always start component names with captial letter. For example, <div /> represents a DOM tag, but <Welcome /> represents a component and requires Welcome to be in scope.

# Components can refer to other components in their output 
```
function App() {
	return (
		<div>
			<Welcome name="Sara" />
			<Welcome name="Carl" />
			<Welcome name="Eddie" />
		</div>
	);
}

ReactDOM.render(
    <App />,
	document.getElementById('root')
);

```

# Props are Read-Only
A component, be it function or class, must never modify its own props. All React components must act like pure functions with respect to their props.

<hr>

## State & Lifecycle
State is similar to props, but its private & fully controlled by the component. But local state is a feature only avail to classes. 

# Converting func to class:
1. Create an ES6 class with the same name that extends React.Component.
2. Add a single empty method to it called render().
3. Move the body of the function into the render() method.
4. Replace props with this.props in the render() body.
5. Delete the remaining empty function declaration.


# Adding local state to class 
1. Replace this.props.date with this.state.date in the render() method
2. Add a class constructor that assigns the initial this.state. Class comp should alw call base constructor with props. 
3. Remove the date prop from the <Clock /> element
```
class Clock extends React.Component {
	constructor(props) {
		super(props);
		this.state = {date: new Date()};
	}
	render() {
		return (
			<div>
				<h1>Hello, world!</h1>
				<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
			</div>
		);
	}
}

ReactDOM.render(
	<Clock />,
	document.getElementById('root')
);
```
