---
layout: post
title:      "Flatiron’s JavaScript Portfolio Project"
date:       2021-01-23 20:32:29 +0000
permalink:  flatiron_s_javascript_portfolio_project
---


For my Rails API JavaScript project, I created a single page application.  My frontend was built using HTML, CSS, and JavaScript, and it communicated with my backend API which I built with Ruby and Rails. All interactions between the client and the server are handled asynchronously and use JSON as the communication format.

My application allows the user to name a study guide (i.e. “SAT Words” or “WWII History Test”) and then create various flashcards of information they’d like to memorize. Upon selecting a study guide, the user can then view the front of all the flashcards they added. In order to see the information they’re trying to memorize on the back of the card, the user simply has to click on the name of the flashcard. Click on the name again, and the information disappears.

I’m particularly proud of this project as it’s the first thing I’ve built that I can see myself using going forward. I might even do so in order to prepare for my assessment. With that being said, this was hands down the most complicated application to complete. I found JavaScript’s fundamentals to be more complex than Ruby’s and the syntax doesn’t exactly scream “syntactic sugar” in my opinion. With that being said, I’ve seen JavaScript referred to as “the crown jewel” of programming languages so I did my best to learn about its various functions. 

One thing I struggled to understand at first was scope. JavaScript has two types of scope: local and global. Scope determines what variables are “visible” or accessible to use. All functions in JS have their own scope (it’s important to remember that objects and functions are considered variables in JavaScript). An example of a local variable would be one defined inside a function.  That variable is scoped only to that function – you cannot call the variable from outside the function. Something cool about this is that because local function variables are only “visible” inside their functions, you can use the same variable name multiple times in different places. Function arguments also work as local variables inside of their functions.  However, variables that have not been declared, even if they exist inside a function, are scoped globally (unless you are running something called “Strict Mode”). Global scope refers to variables declared outside of a function. All scripts and functions can access a global variable. You have to be very careful when creating global variables though as they can overwrite window variables.  

After working on my project and playing around with my code, I’m pleased to say scope is something I’ve come to understand much better. I plan to continue trying to build applications with JavaScript as working on this project made me understand how this language works better than any of the lessons or labs. I’m excited to work on my skills and create more things I find helpful and fun to use.
