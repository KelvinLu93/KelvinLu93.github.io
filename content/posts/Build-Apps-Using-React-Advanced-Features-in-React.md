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

## Lifting state up to higher components

## Code reuse using inheritance

## Code reuse using composition

## Using composition to customize child elts

## Using composition for specialization

## etc
