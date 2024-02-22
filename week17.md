# What is React State
 
* what is state?
* closure review
* reading and setting state
* lifting state up

React.StrictMode calls all your code twice (and thus, everything you console.log will be in the console twice).

* props will always be an object
* props.children is generally used when you want to pass in something that looks like regular html

### What is state?

* state === data
* state belongs to a component (created by the component)
* props come from the outside (ie. from the parent)

```javascript
class Counter {
  constructor() {
    this.count = 0;
  }

  increment() {
    this.count++;
    console.log(this.count);
  }
}

const counter = new Counter();

console.log(counter); // object that has {count: 0}

counter.increment(); // {count: 1}
counter.increment(); // {count: 2}


const increment = () => {
  let count = 0;
  count++;
  console.log(count);
}

increment(); // this function has no memory, so it's not saving the information. It would just result in 1 every time you run it.
```

**closure** - a variable that remembers what a function did after the function has ended. Store a variable outside of the function, and then the function makes reference to it.

```javascript
let count = 0

const increment = () => {
  count++;
}
```

Counter.js
```javascript
import {useState} from 'react';

const Counter = () => {

  const arr = useState(0);
  const getter = arr[0]; // references whatever you passed into useState -> 0 in this case
  const setter = arr[1]; // this is the value that we can change (this value will become the 'getter' whenever this value gets changed)

  const [count, setter] = useState(0); // this one line of code is what we use to replace the above 3 lines of code

  const increment = () => {
    setter(count + 1);
    console.log(count); // if you console.log after you set state, the counter will always be one behind, because count isn't changed until after the function is run again... this console log would show us the getter of the unchanged state
  };

  return (
  <div>
    <h2>Counter component</h2>


    <Display content={count} />
    <button onClick={increment}>Increment!</button> // make sure you dont include the brackets after the function, ie. increment()
  </div>
  );
};

export default Counter;
```

### useState
* hook (functions)
* Feb 2019 hooks were created
  * allowed us to hook into the React lifecycle
* about a dozen hooks built into React, but tens of thousands are available, because people can create their own
* all hooks start with 'use' (how React can tell the difference between hooks and functions, similar to $ in jQuery)
* hooks have to be called inside of the React component
* hooks have to be called unconditionally (ie. not in an if statement) and in the same order every time
* all hook calls will be the first thing you put inside of your function (never inside of a function, loop, if/else, etc.)
* we don't mutate state (don't change values you get from state)
* always use const for getter and setter so that we don't accidentally mutate that data
* call the setter 'setAction', ie. setCount, setUsername, etc.

### When do React functions get called?
* whenever state or props change
* this is why even though we changed the 'setter' variable, when React ran the functions again, the value in 'setter' became the value in 'getter'

* data persists for the amount of time the js is running in the browser
  * therefore, whenever you refresh the page, the data is forgotten

* states can be passed down as props

* Counter - state: count
  * Display - props: count
  * Button - props: setCount

  ```javascript
  const Display = (props) {
    return (
      <div>
        <h2>Displayed count is {props.count}</h2>
      </div>
    )
  }

  export default Display;
  ```

Form.js:
  ```javascript
  import {userState} from 'react';

  const Form = () => {
    const [username, setUsername] = useState('');
    const [password, setPassword] = useState('');

    const submitHandler = (event) => {
      event.preventDefault();

      alert(`you are trying to sign in as ${username} with password: ${password}`)
    }

    return (
      <div>
        <h2>Login Form!</h2>
        <form onSubmit={submitHandler}>
          <label>Username</label>
          <input
            value={username}
            onChange={(event) => { setUsername(event.target.value) }} // event.target.value grabs whatever was typed in by the user
          />
          <br/>
          <label>Password</label>
          <input
            value={password}
            onChange={(event) => { setPassword(event.target.value) }}
          />
          <br/>
          <button type="submit">Login!</button>
        </form>
      </div>
    );
  };

  export default Form;
  
  ```

  ### Lifting State Up

  * App
    * Header - props: count -> How do we get "count" to Header component?
    * Counter - state: count
      * Display - props: count
      * Button - props: increment


* siblings can't talk to each other!
* so we have to go back to the closest common ancestor
* in this example, move state: count up to App.js (just copy paste)
* so very often, all of your state is going to live inside of App since it is the top level


# State Management and Immutable Update Patterns

* mutability vs immutability in JavaScript
* React review
* updating state of varying complexity

### Primitives (Immutables)

* Number
* BigInt
* String
* Boolean
* Symbol
* Null
* Undefined

* **these are unchangeable!**
  * passed by value
  * cannot change value in stored memory

### Complex Data Structures (Mutables)

* objects
* array
* function

* **we are able to change these!**
  * passed by reference
  * storing a reference to the array/object/function
  * we can change their value in stored memory

```javascript
const array1 = [1, 2, 3, 4]
const array2 = array1;

array2.push(5);
array2.push(6);

console.log(array1); // [1, 2, 3, 4, 5, 6]
console.log(array2); // [1, 2, 3, 4, 5, 6]

console.log(array1 === array2); // TRUE - They both point to the same array in memory.

console.log([1, 2, 3] === [1, 2, 3]) // FALSE - They are not the same array, because they are stored in two different places in memory.
```

```javascript
const animals = ['dog', 'cat', 'fish', 'parrot'];
const animalsFull = ['cow', 'horse', ...animals, 'elephant', 'tiger']; // spread operator copies the array into this new array (and places it in an actual new array in memory)
```

* weakness of spread operator:
  * can only do first level of array
  * can't do nested arrays/objects
  * to fix this, you can use JSON.parse(JSON.stringify(array))
    * this creates a new array, turns it all into a string, and then turns it all back to js
    * this removes all nesting!
  * downsides of this JSON method:
    1. Not the most readable...
    2. It can be sort of overkill.
    3. It cannot handle functions/methods.

