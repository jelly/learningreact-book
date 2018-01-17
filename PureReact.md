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

JSX is a way to describe complex DOM trees with attributes, more readable then HTML or XML. In JSX an element's type is
specified with a tag. The tag's attributes represent the properties, the element's children can be added between the
opening and closing tags. JSX also works with components, simply define the component using the class name. Passing an
array to the component can be done by surrounding it with curly braces which is called a JavaScript expression.
Component properties will take two types: either a string or a JavaScript expression which can include arrays, objects
and even functions.

Nested components
```javascript
<UserList>
  <User />
  <User />
</UserList>
```

className is used to define the class attribute
```javascript
<h1 className="react">Title</h1>
```

JavaScript Expressions
```javascript
<h1>{this.props.title}</h1>
<input type="checkbox" defaultChecked={false} />
```

Evaluation - the JavaScript added in between the curly braces will get evaluted.
```javascript
<h1>{this.props.title.toLowerCase().replace}</h1>
```
Mapping arrays to JSX
```javascript
<ul>
  {this.props.items.map((item, i) =>
    <li key={i}>{item}</li>
</ul>
```

## Property Validation

React has built-in automatic property validation for the variable types for a component:

| Type      | Validator
|-----------|-----------------
| Arrays    | React.PropTypes.array
| Boolean   | React.PropTypes.bool
| Functions | React.PropTypes.func
| Numbers   | React.PropTypes.number
| Objects   | React.PropTypes.object
| Strings   | React.PropTypes.string

propTypes can be defined for a class as following:

```javascript
mylist.propTypes = {
   list: PropTypes.array.isRequired
};
```

A warning is thrown when the wrong type is passed.

### Default Props

Default properties can be defined for a component.

```javascript
class ReviewsList extends Component {
  static propTypes = {
     reviews : PropTypes.array.isRequired,
  };
  static defaultProps = {
     reviews: [],
  };
  render() {
    var reviewItems = this.props.reviews.map(function (review) {  // ERROR
      return (
        <RecipeReviewItem key={review.objectId} reviewText={review.reviewText}></RecipeReviewItem>
      )
    });

    return (
      <View>{reviewItems}</View>
    );
  }
}
```

### Custom Property Validation

Custom validation in React is implemented with a function. This function should either return an
error when a specific validation requirement is not met or null when the property is valid.

```javascript
propTypes = {
	subject: (props, propName) =>
		(typeof props[propName] !== 'string') ? new Error('Must be a string') :
			(props[propName].length > 10) ? new Error('Too large') :
				null
}
```

## Refs

A ref is an identifier that React uses to reference DOM elements.

```javascript
class SillyForm extends Component {
  constructor(props) {
   super(props)
   this.submit = this.submit.bind(this)
  }

  submit(e) {
    const { _title, _thing } = this.refs
    e.preventDefault()
    // Do something with submitted data

    // Resset
    _title.value = ''
    _thing.value = ''
    _title.focus()
  }

  render() {
    return (
      <form onSubmit={this.submit}>
        <input ref="_title" type="text" required/>
        <input ref="_thing" type="text" required/>
	<button>Submit</button>
      </form>
    );
  }
}
```

## Inverse Data Flow

A common solution for collecting data from a React component, pass a callback function to the
component as property that the component can use to pass data back as arguments. It's called inverse
data flow becase the component sends data back from the component.

```javascript
const logThing = (title, thing) =>
  console.log(`${title} - ${thing}`)

<ThingForm onNewThing={logThing} />
```
