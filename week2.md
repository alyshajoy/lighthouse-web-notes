# Week Two Notes

### Higher-Order Functions / Functional Programming

* higher-order functions are ones that take in callbacks
  * in other words, higher order functions are any functions which *operate* on other functions
* this means, built in functions (ie. forEach, filter, etc.) can be higher-order functions

* the use of higher-order functions is a huge part of functional programming
* functional programming leads to less bugs, is easier to reason, and takes less time to create, and you can reuse more code

* **_*functions are values*_**
* when using .filter, it won't modify the original array - you need to assign it to a new variable
* using functional programming allows us to reuse small functions all over our code
  * this allows for less code to be written (and less logic required)


### Recursion

* use when the problem you are solving is just a smaller instance of the problem you have already solved


# Lecture

## Primitive Types

* the building blocks of the language
* non-objects
* stored directly in memory (RAM)
* cannot be changed after they are created (immutable)

### 7 Primitive Types

* string
* number
* null
* boolean
* undefined
* symbol
* bigint

* primitives cannot be altered!
  * thus, you can't alter strings (even with .replace) -> you'd have to save the altered string as a new variable, and the first variable would remain the same

  ### Objects

  * arrays
  * objects
  * functions

  * objects are collections of key/value pairs

## Add Items to Arrays and Objects:

  const myArr = [];
  myArr.push(1); // [1]
  myArr[1] = 2 // [1, 2]
  myArr[1] = 5; // [1, 5]

  const studentOne = {};
  studentOne["name"] = "Alice"; // name: "Alice"
  studentOne["name"] = "Bob"; // name:  "Bob"

  const keyName = "age"
  studentOne[keyName] = 42; // name: "Bob", age: 42

  * use const for arrays and objects -> we aren't changing the arrays/objects themselves, we are only changing the contents, so we can use const

  * referencing object keys using [] will work 100% of the time
  * you can only dot into something if it's an object (ie. console.log -> console must be an object)
  * use dot notation every time, unless you cannot
  * you can't dot into an object when you've used a variable for the key name
    * instead you must use bracket syntax

* objects represent a thing
  * keys are adjectives/descriptions about that thing

* arrays are collections of things

* when you pass a primitive to a function, it just passes in a copy of that primitive - it doesn't change the primitive value itself

Example:

const myNum = 10;

const changeMyNum = function(num) {
  num = 20;
  console.log('the num is', num);
};

console.log(myNum); // 10
console.log(changeMyNum(myNum)); // 20
console.log(myNum); // 10

* check out pythontutor.com

* if you pass an object (of any type: array, object, function) into a function, it passes the actual object into the function - not merely a copy
  * so anything you chage in the object with your function directly changes your object outside of the function

### Questions:

* should we be using const functionName = function(){} or function functionName = {};?

## Loop Review

#### For..of

for (const name of names) {
  console.log(name);
}
// returns list of names

#### For..in

for (const index in names) {
  console.log(names[index]);
}
// returns list of names

#### Object Looping: For..in

For...in is the only option here.

* there is no order to the keys in an object
* if you want an order, you need to use an array

for (const key in myObj){
  console.log('key', key);
  console.log('value', myObj[key]);
}
// returns list of keys, then a list of values



## Thursday Lecture - Callbacks

### Functions as Values

**Function Declaration:**
function sayHello(name) {};

**Function Expression:**
const sayHello(name) = function() {};

#### Differences between function declarations and function expressions:

* function expressions are not hoisted (hoisting is the ability to run a function before you declare it in the file)
* function declarations are hoisted to the top of the code

### Callback/Higher-Order Functions

* callbacks are any functions we pass into other functions
* higher order functions are any functions that follow at least one of the following rules:
  * accepts a function as an argument (aka accepts a callback)
  * returns a function as its result

* when passing a function into another function, don't include the () -> that would run the function and put in whatever that function returned, and not the function itself

### Arrow Functions

const sayGoodJob = () => {};
  * like saying: using these values (in the ()), run this code (in the {})

const sayGoodJob = (name) => {
  return `Good job, ${name}`;
}

console.log(sayGoodJob("Alysha")) // Good job, Alysha

* if you have only one parameter in an arrow function, you don't need to include the parenthesis ()
* if only one line, you don't need to include the curly braces {} or return keyword

const sayGoodJob = name => `Good job ${name}`;

* arrow functions are not hoisted


