---
title: "Build Apps Using React Props and State"
date: 2021-07-19T10:41:01-05:00
draft: false
toc: true
categories: [react]
---

Work can be found below:

<https://github.com/HenryFBP/reactjs-learning>

## Props and State

Props and state - data associated with components

### Props

Components need to have a way to receive data from the rest of the app.

And then a way to reference the data in the "render" function of the app.

Then `render()` displays the data.

There are two ways to pass data to a react component:

- props
  - data you pass in from the rest of the app to a component so that the component can use the data to display itself
- state
  - data that is internal to a component

Let's cover props first.

In every React component, you can access the props that have been passed into that component by referencing `this.props`.

'props' are initialization data passed from parent components to child components.

props are read-only and should not be changed by child components.

### State

Every react component is a 'state machine' and can have state.

state is private data, internal to a component.

Props can also be used to initialize the state of a component.

you need to figure out where state should be held in a component tree. You should sparingly choose a few components to have state. The number of components holding state should be as small as possible.

Ideally, the components holding state should be at the top of the tree.

A good rule of thumb to follow is to place state in the highest component possible. Higher level components will calculate the state and pass the info down using props.

## Similarities and differences b/w props and state

### State

- Data private or internal to a component. That belongs to it.
- Your component then renders rich output based on its internal state.
- Completely managed by the component itself and not external apps.
- Components can call functions to update state

State should only ever be modified by calling `setState()`. This is a special function in React.

This is because when you change the state of a component, it must be re-rendered. If you change state without calling `setState()`, the view will not be re-rendered.

Also, state updates may occur in an asynchronous manner. React may bundle together multiple separate state updates and apply them all at once. Do not assume state updates are synchronous.

There is no way to access the previous state except by specifying a special function that takes the previous state as an input argument.

### What should be stored in the state/

- Data that may change with user/server events
- Values which cause the UI to re-render
  - Anything else should be removed or moved to props

### Similarities

- Both props and state are ways to associate data w/ components
- Both are JS objects
- Both trigger a render update on components
- Both are deterministic -- for a specific combo of props+state, the output of a component should always be the same.

### Differences

- Props:
  - Part of a component's configuration from parent
  - Passed from higher-level components down to lower-level components
  - Immutable once received from parent
  - A component cannot change its own props but puts together props for its children
- State:
  - Internal component state, not passed from parent/outside
  - Defined within a component, can take initial values from props
  - Mutable by the component, component is intended to manage its own state
  - Every component can change its own private state, not the state of its children

### Types of components

- Stateless
  - Only has props, no state
  - Simpler and easier to maintain

- Stateful
  - Receive props from parents
  - Manage own state
  - Do client-server comms, process data, and respond to user events.
  - Should be as high as possible on the component tree

## Working with Props

How do we pass data into components?

See demo and code.

Clone this repo:

<https://github.com/HenryFBP/introducing-react-work.git>

And run the below commands in:

`/props-and-state/Props/IntroducingProps/`.

    npm i -g http-server
    http-server .

On line 12:

<https://github.com/HenryFBP/reactjs-learning/blob/c88297b894af58b3911d61f29f7c41852d02f483/props-and-state/Props/IntroducingProps/Props.js#L12>

The "message" is passed as a prop to the `Greeting` component.

## Props with expressions

See code in <https://github.com/HenryFBP/introducing-react-work.git>.

## Transferring Props Manually

See code in this commit: <https://github.com/HenryFBP/reactjs-learning/commit/4c418a419af80f921a4304b6d4e6f71355f612cf>

Note how much repetition of prop transferring there is.

## Transferring props with `...`, aka the spread operator

See commit `0de3cb7623f43a306e2fa90e304020fd572483ed`

## Dynamic types with Props

See commit `e05794d02855e6b93404bd58e452ce3f18ace4b7`

## Default Props

See commit `dc0ca8779e9776d9596482fbbbdb6fd3f68de1de`

## Validating Props

You may want to validate props as JS is a weakly typed lang.

React devs knew validation would be useful, so `prop-types` lib was made.

<https://unpkg.com/browse/prop-types@15.6.2/prop-types.js>

See commit `dfb3f39f8b0d174ca49af3c7f7c83e45bd4d9445`.

Also, uncomment some of the invalid props in `<Student>` object.

## Accessing Children with Props

`props.children` is a special prop passed to components automatically, which allows a component to access its child elements.

See commit `2fd5e8d1920d80bf3563fa4acd4b1a1ffd58f6e8`.

## Using expressions to pass in props values

See commit `4b34885017bf73ec54adfb5cb5b734a9de34a0d0`

## Functions as children

More complicated. Shows you how powerful and "meta" React can be. How you can combine children and expressions to make interesting and intelligent components.

See commit `8b5bd29f176bf4fd45ec676b5d2e8e70c0462b71`

## State

See commit `fa965152a1756788dd1024517ccc40f25b8cc8c6`

## Updating state

See below, and commit `a3ffbcdceaf9e5a4caf8109c02423b1c51e597bf`.

## Event handlers to update state

See commit `2c33bb32343a8b1f64baf6451d430fe6434ff0f8`.

## Accessing previous state

see commit `0af27c333919e290b1d76918686d1ce712ea08fa`