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