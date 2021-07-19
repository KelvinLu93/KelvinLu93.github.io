---
title: "Build Apps Using React: In Development and Production"
date: 2021-07-16T10:41:12-05:00
draft: false
---

## Overview

Use `create-react-app` cli tool, and dev tools, to make an app and make a prod build of that app.

Work can be found below:

<https://github.com/HenryFBP/reactjs-learning>

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

Or this folder:

<https://github.com/HenryFBP/reactjs-learning/tree/master/react-apps-in-dev-and-prod/comment-app-with-state>

## Adding comments

We need event handlers.

```jsx
//App.js

(...)

  addComment = (message) =>{
    this.setState(function(prevState){
      var messages = prevState.messages.concat();
      messages.push(message);
      return {
        messages: messages
      }
    });
  }

```

The act of changing the state re-renders the components.

Next is to wire up the event handler.

See <https://github.com/HenryFBP/reactjs-learning/commit/1b7a0cff1b66eb7f82756c4f7ab8cb645192c2cf> for code.

Note that we use `this.messageInputRef = React.createRef();` in `CommentBox.js` because the input box is an "uncontrolled component", meaning the value in the input box is not controlled using React.Component state. This goes against best practices but is okay because it is a relatively simple input element. This avoids the overhead of managing state and setting up event handlers.

## Deleting comments

See <https://github.com/HenryFBP/reactjs-learning/commit/a43dfe26356d064a598df96a8c2b7658681431fc>.

## React devtools

<https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi>

And then try it out!

## Exploring components with react devtools

Use "Components" view in dev tools and click around.

For profiling, start it and try interacting with the app.

## Prod build

Run

    npm run build

To make a `./build` folder. Feel free to explore the minified files and "manifest" json file.

Also view "index.html" and "css" files. They're minified.

The ".map" files are source code mappings that assist in debugging.

The "main.HASH.chunk.js" file is our app code, so app.js and all components.

## Serving a prod build

Run

    npm install -g serve

    serve -s build/

`-s` redirects 404 to `index.html`.

