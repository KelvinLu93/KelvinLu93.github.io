---
title: "Build Apps Using React: Introducing React for Web Applications"
date: 2021-07-13T12:51:46-04:00
draft: false
---

Skillsoft course, "Build Apps Using React: Introducing React for Web Applications"

Work can be found below:

<https://github.com/HenryFBP/introducing-react-work>

## Overview

React is a declarative, component based JS lib made by FB.

It uses an architectural design called "Virtual DOM" that makes it ideal for apps that work with fast-changing data.

It also uses a declarative extension to JS called JSX, making it easy to control component rendering.

- Virtual DOM
    - Helps React improve rendering by only updating changed components
- JSX
    - Render into HTML
- Babel compiler
    - JSX -> JS

After this course, you'll implement a simple webpage using React APIs.

## Introducing React

In 1990s-2000s, we used static HTML/CSS webpages. No real JS.

In 2000-2010, we started using JS to run clientside code to enhance webpages.

### What is React?

Using plain JS to make web apps is a PITA. Web apps aren't really maintainable with vanilla JS.

- React is a lib that helps web devs make interactive web apps.
- UI building in structured and maintainable way
- Used a LOT in Facebook, built for FB
- Open source project
- Instagram uses React too
- React is relatively new, public in May 2013
    - But very good for making component-based UI

React is NOT a framework.

React is a UI Library. It allows you to build components that are:

- Reusable
- Stateful
- Interactive

### MVC programming paradigm

UI interfaces generally follow the model-view-controller paradigm.

- Model
    - UIs have to work with data used to CRUD data.
    - What is used to represent the data. i.e. DB model, `Person` etc.
- View
    - What the user sees.
    - The data is visualized using the view.
- Controller
    - All of the complex bits that allow the View to work with the Model.

When you build UIs, you split up components to be part of the Model, View, or Controller.

React focuses entirely on the View. It does not interact with the data directly.

It is used to build component-based UIs.

Every UI is made of logical units - called "Components".

The whole idea behind using React is that these Components are reusable. Developed once and meant to be reused across multiple pages and applications.

Simple Components come together in React to make more complex Components.

"Component-driven Development".

## Thinking in React

Let's say we have a UI that looks like this:

```
Search: ________

[ ] Only show products in stock

Name        Price

  *Sporting Goods*
Football    $49.99
Baseball    $9.99
Basketball  $29.99

  *Electronics*
iPod Touch  $99.99
iPhone 5    $399.99
Nexus 7     $199.99
```

You can see we have two categories - electronics and sporting goods.

We can break this UI down into a component hierarchy.

<https://reactjs.org/docs/thinking-in-react.html>

![](/images/Introducing-React-for-Web-Applications/react-components.png)


1. FilterableProductTable (orange): contains the entirety of the example

The entire UI (orange) can be thought of as a top-level component. Lets's call it a `FilterableProductTable`. And it's the root of our component hierarchy.

2. SearchBar (blue): receives all user input

Next, the search bar and checkbox can be another component. It obviously sits inside the `FilterableProductTable` component.


3. ProductTable (green): displays and filters the data collection based on user input

The green table can be thought of as another nested component. This is the actual display of products available. Note that this product table can be further broken down into nested sub components.

For example, "Sporting Goods" and "Electronics" are their own categories and could be individual components, as category headers (4 and 5).

It is optional to you how granular you want to get when using the React JS library.

4. ProductCategoryRow (turquoise): displays a heading for each category
5. ProductRow (red): displays a row for each product

All of these components together build a component tree:

- FilterableProductTable
    - SearchBar
    - ProductTable
        - ProductCategoryRow
        - ProductRow

...

Components are considered "well designed" when when each component has a single responsibility.

This principle of "Single Responsibility" is a classic OOP design pattern. But is also important in component-driven dev't -- each component has 1 responsibility and is reusable.

React is NOT a framework, but a concept and a library built using that concept.

## React Features

- React is a declarative framework

This means that design views are associated with state... As state changes, the views will update automatically.

- React is component-based

Each component manages its own state. It doesn't concern itself with other component's state.

- React is extremely performant

React uses a virtual DOM that gets compared with the actual DOM to avoid re-rendering the DOM, which is slow. React avoids excess DOM updates.

- React apps can be rendered serverside too.

Node facilitates serverside rendering using a one-way data flow.

Data flow is strict - It flows only from the top level components to the bottom level components.

React supports web components, but it also supports React Native for Mobile apps.

## Exploring React Features

### "React is a Declarative Library"

You focus on designing views based on internal component state - That's all you need to worry about.

You just tell React what state you want the UI to be in - This abstracts out attribute manipulation, event handling, and DOM updates!!

Your focus should be on UI design. React handles rendering and updating components based on their state.

This makes code predictable, robust, and easy to debug.

Your responsibility is to say "This is how my UI should look for this state".

### React is "Component-based".

React is component-based, and you need to follow component-driven dev't to code in React.

This means that view components in React have both the logic and the behavior of the view.

This eliminates bridging code where UI changes are relayed to the JS code, which updates the DOM automatically.

Grouping components together makes them manageable and easier to maintain.

### Virtual DOM

DOM rendering slows stuff down. VDOM fixes this. React keeps a copy of every DOM element. This copy is VDOM.

It's not wasteful to do this. It speeds up things.

It's an abstraction that lets us make fast UI changes without actually modifying the base DOM.

React compares changes between the DOM and VDOM during a "reconcilliation process" to see what DOM nodes need to be updated to minimize rendering.

This is all done automatically.

### Serverside rendering

- VDOM enables rendering complete UIs without a browser
- Serverside rendering can make initial page loads very fast

### One-way data flow

React uses composition to build individual view components.

This means that data only flows one way down into individual components.

Data passed to React components flow from parent to child.

Components contain other components within them to build up complex views.

### React Native

This is used to make mobile apps in JS using React.

React libs are separated into:

1. Component libs
2. DOM libs

In React Native,

1. Component libs
2. Mobile app rendering libs

RN lets you build platform agnostic native components with high-perf across multiple mobile platforms.

## The Virtual DOM

Example of a DOM:

- html
    - body
        - h4
        - p
            - img
            - img
        - p
            - div

And React's corresponding in-memory representation:

- React Node
    - React Node
        - React Node
        - React Node
            - React Node
            - React Node
        - React Node
            - React Node

React Nodes are also called "React Elements".

### Events

Events cause Elements to be updated.

- User Events
    - Clicking, swiping, hovering
- Server Events
    - Server responds with data

When the elements get updated, the view changes.

So the model getting updated causes the view to get updated.

## VDOM Usefulness

React first renders the changes to the VDOM.

Then, React will go compare all nodes between DOM and VDOM to see what nodes need to be updated. 

React then only updates what's needed in the new DOM.

This is called "Reconciliation".

This is also totally abstracted away from the user as long as you write React code using best practices.

Note: Virtual DOM is an abstraction/design pattern that React uses, it's not an actual technology.

Note 2: VDOM is NOT a "Shadow DOM" - Shadow DOM is a browser technology designed for scoping variables and CSS in web components.

"React Fiber" is an implementation of React's code algo for fast rendering and updates using incremental rendering.

## Creating a Simple Static HTML page

See <https://github.com/HenryFBP/introducing-react-work/tree/master/react/StaticHTML/FirstReact.html>

"unpkg" is used to host libs.

## Exploring the static HTML page

Download <https://github.com/HenryFBP/introducing-react-work/blob/master/react/StaticHTML/FirstReact.html> and open it with your favorite browser.

Use `CTRL-SHIFT-I` to open Inspect Element. 

Expand the `<body>` tag to see the `<h1>` tag created by the JS.

### Console

Open console.

Notice the devtools extension message.

You can visit <https://unpkg.com/browse/react@16.12.0/umd/> to see the React dev js code. Feel free to read some of the code.

You can also see react-dom at <https://unpkg.com/browse/react-dom@16.12.0/umd/>. You want "react-dom.development.js".

## Referencing prod React libs

See <https://github.com/HenryFBP/introducing-react-work/tree/master/react/StaticHTML/FirstReact_Production.html>.

The only changes are that we use the prod react libs now, line 5/6.

## Nested Elements part 1

See <https://github.com/HenryFBP/introducing-react-work/tree/master/react/CreatingElements/NestedElements.html>. See comments in file. Also run the file to see for yourself.

Tree of elements:

- div#my-react-app
    - var outer_div_el
        -   var el
        -   var another_el

And the resulting HTML once JS runs:

```html
<div id="my-react-app">
    <div id="outer-div-id">
        <div id="inner-div-id">
            <h2 id="header-id">Welcome to react!</h2>
        </div>
        <p id="para-id">React seems rather tedious to work with.</p>
    </div>
</div>
```

Note that there's an error in the console...

```
Warning: Each child in an array or iterator should have a unique "key" prop.

Check the top-level render call using <div>. See https://fb.me/react-warning-keys for more information.
    in div
warningWithoutStack @ react.development.js:316
```

This is because "el" and "another_el" are siblings in the tree:

```js
var element_list = [el, another_el];
```

You *should* specify a key when running `React.createElement` to allow React to uniquely identify each element if they are siblings.

Ex:

```js
var another_el = React.createElement(
    "p", { id: "para-id", key: "key2" },
    "React seems rather tedious to work with.");
```

Here's the fixed version:

<https://github.com/HenryFBP/introducing-react-work/tree/master/react/CreatingElements/NestedElementsWithUniqueKeys.html>

## Nested Elements part 2

<https://github.com/HenryFBP/introducing-react-work/tree/master/react/CreatingElements/Lists.html>

## JSX

So, using `React.createElement` sucks... It is tedious AF. Let's not do that. Instead, JSX is something we can use.

JSX is a lot easier to work with for syntax reasons.

"JavaScript Syntax Extension" is a concise syntax to define a tree structure with attributes. It's an XML-based spec - but with JSX you can make custom tags and components.

You get to write HTML-like syntax that gets used to React so that HTML representation of components can coexist with JS code.

- Large trees representing the DOM are intuitively represented
- Maintains the semantics of JS

## Babel Compiler

JSX looks very similar to HTML. But it's not.

It's written in `<script>` tags and interpreted as JS.

You write code in JS, and then write JSX inside JS.

Example:

```jsx
var some_list = (
    <ul id="nav-id">
        <li>Red</li>
        <li>Green</li>
    </ul>
);
```

It's a good idea to use use parens and semicolon `( ... );` to ensure your JSX code is correct.

Note that this is not HTML.

This is its equivalent JS code, (which would be a pain to write compared to the JSX version):

```js
var some_list = React.createElement(
    "ul",
    { id: "nav-id" },
    React.createElement(
        "li",
        null,
        "Red"
    ),
    React.createElement(
        "li",
        null,
        "Green"
    )
);
```

Browsers do not understand JSX. So any JSX inside JS has to be compiled/translated to a form the browser understands.

We need to use Babel, a compiler, to turn JSX into JS.

React is compatible with features of ECMAScript6, or ES6. FYI, not all browsers support ES6... Babel can also turn ES6 into ES5, which is much more widely supported.

Babel can turn JSX into JS either:
- In dev mode in the browser, which is highly inefficient, or
- At build time in prod environments, which is faster

## JSX + Babel

Run this file in your browser.

<https://github.com/HenryFBP/introducing-react-work/blob/master/react/JSXBabel/SimpleJSX.html>

```jsx
ReactDOM.render(
    <h1>Welcome to react!</h1>,
    document.getElementById("my-react-app")
)
```

So much simpler.

## More JSX

Our lists example, converted.

Run <https://github.com/HenryFBP/introducing-react-work/blob/master/react/JSXBabel/ListsJSX.html>.

## Simple Expressions with JSX

The previous example, but showing how to use "expressions" (string templates) in your JSX code.

Run <https://github.com/HenryFBP/introducing-react-work/blob/master/react/JSXBabel/SimpleExpressionsJSX.html>.

## More Expressions with JSX

Function evaluation, `<img>` tags within elements.

Run <https://github.com/HenryFBP/introducing-react-work/blob/master/react/JSXBabel/MoreExpressionsJSX.html>. Make sure the "images" folder is reachable relative to the HTML file.

## Summary

