---
title: "Build Apps Using React: In Development and Production"
date: 2021-07-16T10:41:12-05:00
draft: false
---

## Overview

Use `create-react-app` cli tool, and dev tools, to make an app and make a prod build of that app.

## Setting up a Simple Comment Application  

Install deps:

    npm i -g create-react-app npx

Install a react app with JSX, a Babel compiler, and WebPack for asset bundling/minification:

    npx create-react-app comment-app

Start the app:

    cd comment-app
    npm start

You can see that in `src/index.js`, this is where the "<App/>" element is defined, from `App.js`.

See "Comment.js", a new component we make:

<https://github.com/HenryFBP/reactjs-learning/tree/master/react-apps-in-dev-and-prod/comment-app-structure-only>

And "CommentList" is a list of Comment components.

The above repo has no state or event handlers, just basic HTML structure.

If you get errors trying to run `npm start`, just delete `node_modules/` and run `npm install` and the `npm start`.

## Adding State to the app

Now to add state.

See <https://github.com/HenryFBP/reactjs-learning/tree/master/react-apps-in-dev-and-prod/comment-app-with-state>.

You store the state in the highest component possible, which is "App" in our case.

State should also flow from higher level components down to lower level components via properties.

See this commit: <https://github.com/HenryFBP/reactjs-learning/commit/9a037e6d415f0ae49cddb5aa04dfe5fcbef62265>

## Adding comments

## Deleting comments

## React devtools

## Exploring components with react devtools

## Prod build

## Exploring files in prod build

## Serving a prod build

## Summary