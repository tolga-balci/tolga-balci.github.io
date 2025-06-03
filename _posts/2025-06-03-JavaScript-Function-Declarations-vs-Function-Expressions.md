---
layout: post
title:  "JavaScript: Function Declarations vs Function Expressions"
date:   2025-06-03 15:53:18 +0300
description: "My detailed notes on function declarations, function expressions and arrow functions, their comparison, and their use cases."
categories: JavaScript
---

I am working on my JavaScript notes and will publish the ones that I think will benefit a larger audience. 

This post is my notes on function declarations, function expressions and arrow functions and when to use each one.

<!--more-->

## The Core Difference

- **Function Declaration**: You're telling JavaScript "here's a function that exists"
- **Function Expression**: You're creating a function and storing it somewhere (usually in a variable)

## Basic Syntax Comparison

| Aspect        | Function Declaration        | Function Expression                  |
| :------------ | --------------------------- | ------------------------------------ |
| Syntax        | `function myFunction() { }` | `const myFunction = function() { }`  |
| Keyword first | Yes, starts with function   | No, starts with variable declaration |
| Name required | Yes, must have a name       | No, can be anonymous                 |
| Semicolon     | No semicolon needed         | Semicolon needed (it's a statement)  |

## Simple Examples

{% highlight javaScript %}
// Function Declaration
function sayHelloDeclaration() {
    return "Hello from declaration!";
}

// Function Expression
const sayHelloExpression = function() {
    return "Hello from expression!";
};

// Both work the same way when called
console.log(sayHelloDeclaration()); // "Hello from declaration!"
console.log(sayHelloExpression());  // "Hello from expression!"
{% endhighlight %}

## The Key Behavioral Difference: Hoisting

This is where they behave completely differently:

{% highlight javaScript %}
// This works - you can call it BEFORE it's defined
console.log(callMeEarly()); // "I was called early!"

function callMeEarly() {
    return "I was called early!";
}

// This breaks - you CANNOT call it before it's defined
console.log(callMeLate()); // ERROR: Cannot access 'callMeLate' before initialization

const callMeLate = function() {
    return "I was called late!";
};
{% endhighlight %}

**Why this happens**: JavaScript hoists function declarations to the top of their scope, but function expressions are _not_ hoisted.

## When to Use Each One

| Situation                                              | Use This             | Why                                                |
| :----------------------------------------------------- | -------------------- | -------------------------------------------------- |
| General functions you'll call throughout your code     | Function Declaration | Hoisting makes them available everywhere in scope  |
| Functions that depend on conditions or other variables | Function Expression  | More predictable - can't be called before creation |
| Functions you want to pass around as values        | Function Expression  | Clearer that it's a value being assigned           |
| Functions inside objects or arrays                 | Function Expression  | Required syntax                                    |
| Callback functions                                 | Function Expression  | More common pattern                                |

## Practical Examples

### Use Function Declaration When:


{% highlight javaScript %}
// Main functions that other code depends on
function calculateTax(amount, rate) {
    return amount * rate;
}

function formatCurrency(amount) {
    return `$${amount.toFixed(2)}`;
}

// These can be called from anywhere in this scope
const total = calculateTax(100, 0.08);
const formatted = formatCurrency(total);
{% endhighlight %}

### Use Function Expression When:


{% highlight javaScript %}
// Conditional function creation
let mathOperation;

if (userWantsAddition) {
    mathOperation = function(a, b) {
        return a + b;
    };
} else {
    mathOperation = function(a, b) {
        return a * b;
    };
}

// Callback functions
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(function(number) {
    return number * 2;
});

// Functions as object properties
const calculator = {
    add: function(a, b) {
        return a + b;
    },
    subtract: function(a, b) {
        return a - b;
    }
};
{% endhighlight %}

## Arrow Functions (A Special Type of Function Expression)

Arrow functions are always function expressions:


{% highlight javaScript %}
// Traditional function expression
const addTraditional = function(a, b) {
    return a + b;
};

// Arrow function expression
const addArrow = (a, b) => {
    return a + b;
};

// Short arrow function (implicit return)
const addShort = (a, b) => a + b;
{% endhighlight %}


## Complete Comparison Table

| Feature             | Function Declaration | Function Expression     | Arrow Function          |
| :------------------ | -------------------- | ----------------------- | ----------------------- |
| Hoisting            | Yes, fully hoisted   | No, not hoisted         | No, not hoisted         |
| Can be anonymous    | No, needs a name     | Yes                     | Yes                     |
| Has own `this`      | Yes                  | Yes                     | No, inherits `this`     |
| Can be constructors | Yes                  | Yes                     | No                      |
| Block scope         | No, function scoped  | Yes, if using let/const | Yes, if using let/const |

## When Each Is Required

**Must use Function Expression for:**

*   Object methods (modern way)
*   Array callbacks
*   Conditional function creation
*   IIFE (Immediately Invoked Function Expressions)

**Must use Function Declaration for:**

*   When you need hoisting behavior
*   Recursive functions that reference themselves by name
*   When creating constructors (though classes are preferred now)

The simple rule of thumb: Use function declarations for your main, standalone functions. Use function expressions for functions that are values, callbacks, or conditionally created.
