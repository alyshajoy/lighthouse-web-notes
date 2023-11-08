# Asynchronous Control Flow Lecture

### Sync Code

* code that runs in the order it is written
* can block

* **async code** is non-blocking code
  * it steps out of the way and allows other code to be processed

* setTimeout(() => [], milliseconds);
  * setTimeout tells Node to hang onto the callback function you insert, and tells it to run it at some point in the future
  * will run *at least* as many milliseconds later that you put in -> is not always exact

* leading underscores (ie. _methodName) indicate that it is meant to be private (ie. don't change it! - pretend you have no access to it)

### Async Code

* async code cannot run until all of the sync code has finished running
* async functions generally do not have return values
* node won't capture return values from functions given in setTimeout
* to capture values from async code, you need to put what you want returned into a callback's argument

#### setInterval()

setInterval(() => {
  console.log('hello');
}, 1000);
  * would print "hello" to console log every second -> would run until the end of time
  * use control C to kill any running program
  * use an if statement with clearInterval() to tell it when to stop

### File System Functions ('fs')

  * package that is built into Node
  * fs.readFile(path, options object, callback(error, data))

const fs = require('fs')
  * imports into your file
  
```javascript
const fileContents = fs.readFile('./fileName', {encoding: 'utf8'}, (err, data) => {
  if (err) {
    // an error occured
  } else {
    console.log(data.length); // or whatever else you want to do with the data
  }
});
```
  * adding the encoding option turns it from binary into a readable format