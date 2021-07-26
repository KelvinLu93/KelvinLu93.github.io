---
title: "Build Apps Using React: Advanced Features in React"
date: 2021-07-21T12:47:58-05:00
draft: false
toc: true
categories: [react]
---

Work can be found below:

<https://github.com/HenryFBP/reactjs-learning>

## Lists without keys

See commit `5c195b3be2f21b825eb93630919e8f16c7f2b5e6`.

## Unique Keys for lists

see commit `897258b579322daa5b80cd4d767ace2753f91c54`

## Correct key usage

See commit `7e5a54395b3b56b197b65763b636ffd7d01b493c`

## Rendering using conditional if

see commit `30deea9f99491f2fbf6501b8748bab6dbc64060f`

## Conditional Rendering using variables

see commit `84afdf0de4687442e95595522c744d810ebeec2c`

## Conditional Rendering using inline logical ops

see commit `731091a54d4bec6cd7573297615f55bedfec7f00`

## Ternary ops and preventing rendering

see commit `4897df902a9577df74dd53e14ba3ac06845ebb8a`

## Local state (don't do this)

> Where exactly should state be stored when you have a component tree?

In the HIGHEST POSSIBLE component in the component tree.

> Which components should only receive props?

State should be lifted into a higher level component.

It should be as high up as possible in your component tree.

This lets you sync your state across multiple components in your app.

In this example, we intentionally DON'T lift state up, and see how bad it is to do this.

See commit `9a9fcf7a9e12c87616e43d707afb0d862132590c` for bad practice example.

### Disadvantages

See commit `e4bc6007bdc1c25f1380eb943d6bf3f51c64823c` for addition of a 2nd currency, rupees.

There's a problem - Now that we've added rupees, how do we convert between the 2 currencies?

There's no conversion going on between dollars and rupees.

Let's say 70rup = 1usd.

## Removing state from lower level components

Incomplete code as we removed the state but didn't lift it up yet.

Commit `23cb98bfca74254437c2c3d3a67ab3d77e7fb3d1`.

## Lifting state up to higher components

See commit `9f7c74a39af8f91372861b93d6fe558bd6142c52`.

## Code reuse using inheritance

Ideally, don't duplicate functionality if you can avoid it.

Like this commit: `13dc5d20109553f8a9200b7082ee83899efdd6cc`.

Use inheritance to allow for code reuse, especially in React.

In React, it's preferable to use "composition" instead of "inheritance" to reuse code.

In Java, common practice is to make a "base class" and several "derived classes". (this is inheritance.)

In React, it's possible to use inhertiance to reuse code, but it's not preferred.

In this example, we use inheritance, but also will later see why Composition is BETTER than inheritance for code reuse when using React.

See `e3f3860ee0693fefbc60d4b7c027950ff03783b1`.

## Code reuse using composition

See `ec5effbc509428f0d54a415b7dc83e7fea4a133b`. Much simpler.

## Using composition to customize child elts

Composition is all about containment. You use composition to enforce design principles on the components that make up your application.

See `155647245eb6844171a0c047835b66ebb9fea248`.

## Using composition for specialization

Generally, in OOP like in Java, you often think of inheritance for specialization.

The base class is more generic, and derived classes are more specific.

i.e.:

- Shape
    -   Square
        - Rectangle
    -   Triange
        -   EquilateralTriangle
    -   Circle

In React JS, you'll use composition for specialization.

You make a more generic component that can take a number of values as input via props. You'll then customize this component using props to use for specialized cases.

i.e. a specific component that calls a generic component and customizes it by passing in props.

See commit `0adff014dae89f12363be2fe01f9cc852b8c4e14`

## Global properties w/o context

Best practices in React dictates that data flows from higher level components down to lower level components via props.

In React, state should be as high as possible and props get passed down to components for the purpose of rendering them.

However, if you have a property specified across your app, and/or have a bunch of nested components within your app that need that property to render, update their state, or change their behavior.

Passing a property like this through the nested component tree may be hard or impossible.

What do we do in this situation?

React lets us use a "Context" to let props flow through components in the tree without passing them manually.

But first, an example of a simple app WITHOUT context, where we must manually pass down properties that are applicable to many components.

See commit `b6d55cf61aa75f501f65e4905f4a85b177111567`.

## Using context to specify global properties

If you have values that need to be propogated to a lot of components in arbitrary levels, and it's messy if you use props (i.e. repetetive), then you should use "Context" in React.

Context lets you pass data through the component tree without manually passing it through props.

Context is a good idea to use for:

- Themes
- Locales
- Other data that generally applies to the entire application

See commit `51b331c559be0f3a08fa902a8c3a10d1792c944c`.

And then see commit `e0c2a9c040e0d814e86365f5971aca068b06d184` for an example of a "Context Provider", a way to specify a value for SOME context that can be consumed by child components. 
