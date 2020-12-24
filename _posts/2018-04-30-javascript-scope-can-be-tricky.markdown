---
date: "2018-04-30"
title: "JavaScript Scope Can Be Tricky"
tags: [javascript]
---

I was writing some vanilla JavaScript this weekend - it has been quite awhile since I did so. As I've been writing mostly C# code lately, what always gets me with JavaScript isn't its pain points or subtle nuances. It's the different behavior from compiled languages I'm often used to, like C# or Java. No matter how often I work in JS, as long as I'm committed to being a polyglot, it'll trip me up to the point that it's just a little bit of an annoyance.

Take this seemingly simple JavaScript code. What will be output to the browser console?

```javascript
var myString = 'I am outside the function';
function myFunction() {
    console.log(myString);
    var myString = 'I am inside the function';
};
myFunction();
```

Was your guess `I'm outside the function`? Or was it `I'm inside the function`? I hate to let you down, but in either case you would be incorrect.

In your favorite browser's developer tools, the console will log `undefined`. But why?

In JavaScript, functions create brand new scopes. If we condense the example a little:

```javascript
function myFunction() {
    // new scope, the variable is inaccessible outside the function
    var myString = 'I am inside the function';
}
myFunction();
console.log(myString); // undefined!
```

Of course, if you've worked in JavaScript (or basically any programming language) you can't expect to declare a variable without an assignment and expect to get anything back but `null` or, in JavaScript's case, `undefined` when I do something like this:

```javascript
var dave;
console.log(dave); // will log undefined
```

So, if we look at this again:

```javascript
var myString = 'I am outside the function';
function myFunction() {
    console.log(myString);
    var myString = 'I am inside the function';
};
myFunction();
```

To take all we've learned, we need to be aware of how something called [hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting) works. For our purposes just know this: variable declarations in JS are always hoisted to the top of the current scope -- variable assignments **are not**.

So here, the declaration is hoisted to the top of the scope - or, in our case, the `myFunction()` function. So before the `console.log` statement in the function, you can basically envision a `var myString` in the first line of the function. And, of course, knowing this now it is clear to see why the logging statement will come back as `undefined`.

Just remember this: **always** define your variables at the top of the **current** scope. Always.
