---
title: "React Notes: Introducing React"
date: 2021-07-13T12:51:46-04:00
draft: false
---

Skillsoft course.

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

### Thinking in React
### React Features
### Exploting React Features
### The Virtual DOM
### Creating a Simple Static HTML page
### Exploring static HTML page
### Referencing prod React libs
### Nested Elements part 1
### Nested Elements part 2
### JSX
### Babel Compiler
### JSX + Babel
### More JSX
### Simple Expressions with JSX
### More Expressions with JSX
### Summary