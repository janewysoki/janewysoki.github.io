---
layout: post
title:      "Flatiron's React/Redux Portfolio Project"
date:       2021-03-29 14:49:42 -0400
permalink:  flatirons_react_redux_portfolio_project
---


I just finished my fifth (and last) project for the Flatiron School. I was the busiest I’ve ever been at my full-time job and also balanced learning the hardest section we’ve had yet at school (JSX, Redux, React). I may not be as good at React and Redux as I’d be without having to work long hours, but I now know for certain I have the drive to become great. I had some sleepless nights watching various study sessions and working my way through the curriculum, but I always looked forward to making the switch every night from my legal career to my coding lessons. It makes me feel confident I’ve chosen the right path. 

Overall, this module really challenged my knowledge of JavaScript fundamentals. I had to make sure I understood the basics of JS before I could begin to comprehend React, and I had to make sure I knew React quite well before knowing how to do anything with Redux.

For example, in order to understand state in React, I had to learn that there are limitations of props in JS. For a component’s props to change, the PARENT component needs to send it new props. Then the component would remake itself with those new props sent by the parent. Once I fully understood this, I could see that state in React had a purpose - to provide us with a way to maintain and update data WITHOUT requiring the parent to send that new information. State in React is data that is mutated in your component and when state is modified, the changes automatically trickle down to the child components via props. It’s a way to keep track of values that are expected to change. In React, the initial state is assigned in the constructor and it gets updated with `setState()`, which is provided to us by React. These are all things I made sure I fully understood when it came time to understand state in Redux. By doing so, I was able to understand why state in Redux could be useful - because it makes changes predictable. Redux is actually a “state manager” for React. The main goal of Redux is to make state global which is achieved by storing it in our Store, an object tree/global location/POJO that comes with methods such as `getState()` and `dispatch()`. Redux introduces many new words and concepts (i.e. action creators, reducers, dispatch, etc.), but the general flow begins with initializing the store with the `createStore()` method. If a user triggers an event (i.e. clicking a button), then an action gets dispatched and gets sent to the reducer. The reducer takes in the current state from the incoming action and then builds a new piece of state. 

This module, more so than the others, made me realize the importance of staying up to date on what I’ve learned prior to now as it all just builds on each other.

