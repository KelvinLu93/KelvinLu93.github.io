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



## Global properties w/o context

## Using context to specify global properties

## Summary