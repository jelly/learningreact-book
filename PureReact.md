# Pure React

React is split up in two libraries, React is the library for creating views and ReactDOM is the library used to actually
render the UI in the browser.

## The Virtual DOM

HTML is simply a set of instructions that a browser follows when constructing the DOM (document object model). The
elements that make up an HTML document become DOM elements when the browser loads HTML and renders the user interface.

The DOM API is a collection of ojbects that JavaScript can use to interact with the browser to modify the DOM. The DOM
API is slow when adding new DOM elements and interacting with the DOM efficiently is quite painful from JavaScript. This
is where React comes in, instead of interacting with the DOM API you interact with a virtual DOM provided by React.

The virtual DOM is made up of React elements, which are JavaScript objects. When changing a React element, we change the
virtual DOM and React will render those changes using the DOM API as efficiently as possible.

## React elements

A React element is a description of what a DOM element should look like.

```javascript
React.createElement("h1", null, "Title");
React.createElement("h1", {id: "title", data-type: "title"}, "Title");
```

## ReactDOM

The ReactDOM contains all the tools to geenrate HTML from the virtual DOM. Rendering a React element including it's
children to the DOM can be done using ReactDOM.render.

```javascript
const elem = React.createElement("h1", null, "Title")
ReactDOM.render(elem, document.getElementedById('react-container'))
```

## Children

A React element can have children by passing the children as third argument to React.createElement.

```javascript
const elem = React.createElement(
		"ul",
		null,
		React.createElement("li", null, "One"),
		React.createElement("li", null, "Two"),
		React.createElement("li", null, "Three")
)
```

The elem object will have a 'props.children' property which is an array of React elements.

## React Components

React Components are re-usable DOM structures of a user interface, they may take different data.

## React.createClass

Although React.createClass is indicated as deprecated some projects might still use it, the new method is using ES 6
classes and extending React.Component. With React.createClass we can create a re-usable React component.

```javascript
const mylist = React.createClass({
		displayName: "myList",
		render() {
			return React.createElement("ul", {className: "mylist"},
					this.props.items.map((item, i) =>
						React.createElement("li", { key: i }, item)
					)
			)
		}
})
const items = ['One', 'Two', 'Three']
ReactDOM.render(
	React.createElement(mylist, {items}, null),
	document.getElementById('react-container')
)
```

Or as ES 6 Class (recommended).

```javascript
class mylist extends React.Component {
  constructor(props) {
  	super(props);
  }
  render() {
   	return React.createElement("ul", {className: "mylist"},
					this.props.items.map((item, i) =>
						React.createElement("li", { key: i }, item)
					)
			)

  }
}
```

## Stateless Functional Components

Stateless functional components are functions that take in properties and return a DOM element. When creating such a
component you should strive to make such a component a pure function, they should take in props and return a DOM element
without side effects. If you need to encapsulate functionality or have a this scope, you can't use a stateless
functional component.

```javascript
const mylist = ({items}) =>
  React.createElement("ul", {className: "mylist"},
    items.map((item, i) =>
      React.createElement("li", {key: i}, item);
    )
 )
```

## DOM Rendering

ReactDOM.render renders the UI efficiently every time we change the state, it will always try to keep DOM insertions at a
minimum since these are quite costly. ReactDOM will try to make changes by updating DOM elements instead of removing for
example li's in an ul and insertion new li's, it will update the already drawn li's.

## Factories

A factory is a special object which can be used to abstract away the details of instantiating objects. In React
favorites are used to help us create React element instances. React has built-in factories for all commonly supported
HTML and SVG DOM elements and you can use React.createFactory to create your own. For example for a creating a heading
React has a factory.

```javascript
React.DOM.h1(null, "My Title")

const items = ['One', 'Two', 'Three']
const mylist = React.DOM.ul(
  { className: "mylist" },
  items.map((item, key) =>
    React.DOM.li({key}, item)
  )
)
```

## React Element as JSX
