# Data Fetching and Other Side Effects

* Pure Functions
* Side-Effects
* React Hooks
* The `useEffect` Hook

### Pure Functions

Typically in JS, we're used to procedural programming
* blocks, scope, variables
* `if`, `while`, `for`, etc.

In functional programming, most/all functions should be ***pure*** in nature.

A `pure` function abides by two major rules:
1. Identical return value given identical arguments.
2. No side effects.

### Side Effects

If a function mutates a variable or value outside of its own blocked scope, it is said to have a side effect.
* performing standard input-output operations
* updating the value of a variable defined outside of the function
* mutating an argument of the function that was passed in by reference (all complex data structures)
* envoking a separate function that itself has side effects

**Examples of Impure Functions:**

```javascript
let sumValue = 0; // This is defined outside of our function.

const sum = (num1, num2) => {
  sumValue += num1 + num2; // We're working with an outside value - impure!
  return sumValue;
};
```
```javascript
const greeting = (name) => {
  console.log(`Hello, ${name}!`); // uses console.log (performs output operations) - impure!
}

greeting("Alysha"); // Hello, Alysha!
```

**Examples of Pure Functions:**

```javascript
const sum = (num1, num2) => {
  let sum;
  sum = num1 + num2;
  return sum;
}
```
```javascript
const greeting = (name) => {
  return `Hello, ${name}!`; // not reliant on things outside the function (like console) - pure!
}
```

## React Hooks

Hooks we've used so far:
* `useState`
* `useReducer`

Rules when using React hooks:
1. Only call hooks at the top level of your function/component.
2. Only call hooks in React functions (in a component).

## The `useEffect` Hook

This is a React hook meant to help facilitate side effects in our React applications.

If you're working on a component and end up working on something that would make your function "impure", consider using the `useEffect` hook.

Common use cases in React:
* timers (`setInterval` or `setTimeout`)
* setting up a subscription (subscribed to changes of an outside value)
* direct updates to the DOM (esp. outside of your component and/or React App)
* fetching data from an external resource (API endpoints)
* console logging

## Syntax

```js
// ... inside of a React component...
import { useEffect } from 'react';

// Envoke the useEffect hook supplied from React:
useEffect(() => {  // looks for 2 arguments: 1. function 2. dependencies array
  // In our callback belongs our side-effect code.
}, []); // Dependencies Array determines how often our side effect runs.

// 1. If no dependencies array is provided, the side-effect runs EVERY render.
// 2. If the dependencies array is empty, the side-effect function runs ONCE on first render.
// 3. If any values in the dependencies array change, the side effect runs again.
// 4. Optionally, you can return a "clean-up" function!
```

**`useEffect` Example:**

```js
// TitleComponent.js

import React, { useState, useEffect } from 'react';

const TitleComponent = () => {

  const [title, setTitle] = useState('My React App');

  // document.title = '???'
  useEffect(() => {
    document.title = title;
  }, [title]);

  return (
    <section>
      <h1>{title}</h1>
      <input 
        type="text"
        onChange={e => setTitle(e.target.value)}
        value={title}
      />
    </section>
  );
};
```

```js
// IntervalCounter.js

import React, { useState, useEffect } from 'react';

const IntervalCounter = () => {

  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const secondsInterval = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);

    // Clean-up function! React knows it's a clean up function if we RETURN it!
    return () => {
      clearInterval(secondsInterval);
    };
  }, []);

  return (
    <section>
      <h2>Interval Counter</h2>
      <p>{seconds} seconds have passed.</p>
    </section>
  );

}


export default IntervalCounter;
```