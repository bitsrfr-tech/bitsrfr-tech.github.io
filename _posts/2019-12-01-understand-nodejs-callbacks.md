---
title: Understand Node.js Callbacks
date: 2020-04-01
---

# Understand Node.js Callbacks

When you understand Node.js callbacks, you might start preferring asynchronous Node.js functions. Once you go callback, you never go back.

## What Does Asynchronous Mean?

You are cooking spaghetti. You’ve got noodles, sauce, and parmesan cheese. You have to cook things in a certain order, or the spaghetti won’t turn out right.

You have to boil the noodles, then heat up the sauce, then grate the cheese, then strain the noodles, then put the sauce on the noodles, then scoop the spaghetti onto a plate, then sprinkle the grated cheese on top.

For many of the steps above, order matters. For example, if you sprinkle the grated cheese on top of the noodles before you boil them, things are going to get weird. If you try to strain the noodles before you boil them, you’re going to get an error.

In many programming languages, the computer reads the instructions from top to bottom, not moving to the next line until the current line has executed. That sounds nice, comfortable, simple. But, going back to the spaghetti cooking, imagine not being able to move to the next step until the current step is finished. That means you can’t do anything except the step that you’re on. You can’t heat up the sauce while you are boiling the noodles. You can’t grate the cheese while the sauce is heating up. You have to just sit there twiddling your thumbs while the noodles are boiling, and then again while the sauce is heating up. That’s what synchronous programming is like. It’s not very efficient.

In Node.js, and some other modern languages, things can be asynchronous — the computer can do multiple things at the same time. When you look at the way humans work, we are asynchronous by nature. We don’t sit idly by waiting for the noodles to cook. We put them on the stove, then go work on another task until the noodles are ready. Then we come back to the noodles. That’s asynchronous.

Computers can be asynchronous too. A program can call a function, and while that function is being processed, it can start working on another function that doesn’t rely the first function.

## What is a Callback?

When you cook your noodles, you have some mechanism to tell you when they’re done. It might be a timer or it might just be your intuition. A callback is the mechanism in a program that tells the computer when something is done.

The key to understanding Node.js callbacks is to understand that Node.js is event-based. When you use Node.js asynchronously, a function only runs when an event triggers it.

Instead of cooking the spaghetti line-by-line, you cook the spaghetti based on events. When the timer dings (an event) to indicate the sauce has finished heating up, you pour the sauce on the noodles. That’s how Node.js works.

Callbacks are kind of like timer dings. When a function is finished running, the callback function runs. Here is what part of our spaghetti cooking might look like in Node.js.

    var temp; // temperature at which water boils
    var timeToBoil = 7; // how many minutes to boil the water

    function heatWater(temp, callback) {
        while (temp < 212) {
            // do nothing, just keep looping until temp = 212
        }
        callback;
    }

    function boilNoodles(time) {
        for (i = 0, i < time, i++) {
            console.log("Water is still boiling");
        }
    }

    heatWater(temp, boilNoodles(timeToBoil));

There are two functions in the code snippet: heatWater and boilNoodles. The heatWater function has a callback to the boilNoodles function. So when the heatWater function finishes (an event), the boilNoodles function is triggered. Here is an up-close of the heatWater function and callback.

    function heatWater(temp, boilNoodles(timeToBoil) {
        ...
    }

The boilNoodles function doesn’t run until the callback triggers it.

Callbacks are that simple. It’s just a function triggering another function when it is finished doing its job. As with any language, there are some additional ways to write callbacks, and there are some standard ways of doing things within Node.js callbacks, like handling errors.

## Further Reading

Now that you have a basic understanding of callbacks, I suggest reading about some of the best practices and ways of doing callbacks in the following articles:
[**How to Write Callbacks in Node.js**
*Callbacks: They sound difficult, but let's take a moment to explain why they're not.*medium.com](https://medium.com/better-programming/callbacks-in-node-js-how-why-when-ac293f0403ca)
[**Callback Hell**
*Callback Hell *A guide to writing asynchronous JavaScript programs* ### What is "*callback hell*"? Asynchronous…*callbackhell.com](http://callbackhell.com/)
[**Callback function**
*A callback function is a function passed into another function as an argument, which is then invoked inside the outer…*developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)
