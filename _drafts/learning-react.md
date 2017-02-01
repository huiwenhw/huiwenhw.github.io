http://reactfordesigners.com/labs/reactjs-introduction-for-people-who-know-just-enough-jquery-to-get-by/

Coding in react
In React, we'll write in a special syntax called JSX, which lets you put HTML-like code inside Javascript. When React processes the code, it automatically converts the HTML part inside JSX into valid JavaScript code so that the browser can understand. 

```
var TweetBox = React.createClass({
	render: function() {
	}
});

ReactDOM.render(
	<TweetBox />,
    document.getElementById("container")
);
```

React.createClass creates a piece of UI with a name (TweetBox) & gets attached to DOM through ReactDOM.render(<TweetBox />, document.getElementById("container"))
==> This TweetBox UI is added inside the <div id="container"> tag. 

Constructing UI 
To actually construct the UI, we've to fill in the render() method

```
var TweetBox = React.createClass({
	render: function() {
		return (
			<div>
				Hello World!
			</div>
		);
	}
});
```
==> Note, there must be only one outer-most tag inside return ( ... );

Attaching UI to DOM 

```
ReactDOM.render(
	<TweetBox />,
    document.getElementById("container")
);
```

ReactDOM.render takes 2 args 
Variable Name: <TweetBox />,
& DOM object: document.getElementById("container")

Handling change event 
In React, we Write the event handler as a method 

```
React.createClass({
	handleChange: function(event) {
		console.log(event.target.value);
	},
	render: function() {
		...
		<textarea className="form-control"
			onChange={this.handleChange}></textarea>
		...		  
	}
});
```

==> We use {} syntax to include any JavaScript code inside HTML part of JSX like: {this.handleChange}. We use this cause its a method on this UI obj. 
==> We use console.log to check if handler is being called. Event obj contains target = textarea & value gives curr val of textarea. 

State (Diff btwn jQuery & React) 
# In jQuery, we modify the DOM directy when some event happens 
# In React, we dont modify the DOM directly. Instead, we modify something called the "state" in an event handler, done by this.setState. Every time the state is updated, render() is called again. Inside render(), we can access the state. 

```
React.createClass({
	handler: function() {
		this.setState({ ... })
	},
	render: function() {
		// Use updated state 
	};
});
```

==> That's how we update the UI in response to an event. 

For example, lets say we want to watch for text changes in the textbox:
A few things to get the Event Handler working
1. Initialise the state object by:
	- Using getInitialState method & let it return a JS obj 
	- which becomes the initial state 
2. Modify event handler to set text field to the val in the textbox 
3. Check that the state is correctly set by using debugging code. Remove it afterwards. 

```
React.createClass({
	getInitialState: function() { // 1. 
		return {
			text: ""
		};
	},
	handleChange: function() {
		this.setState({ text: event.target.value }) // 2.
	},
	render: function() {
		return (
			<div className="well clearfix">
			<textarea className="form-control" 
					  onChange={this.handleChange}>
			</textarea>
			<br/>
			<button className="btn btn-primary pull-right" disabled> Tweet </button>
			<br/> 				// 3. 
			{this.state.text} 	// 3.
		)
	};
});
```
Using if in React 
We can't write if/else inside the { } brackets. We can use a ternary expression like (a ? b : c). If we need a longer if/else, we have to use a method:

```
method: function() {
	... 
},
render: function() {
	<span>{ this.method() }</span> 
}
```

Using logic in React 
We want to disable the tweet button if there's no text. We do it with the {} brackets:

```
<button className="btn btn-primary pull-right"
        disabled={this.state.text.length === 0}>Tweet</button>
```

Diff btwn calling a method && using event handlers 
# Calling method: { this.method() } 
# Calling event handlers: onClick={ this.handler } && assign to attributes like onChange / onClick

