### Day Two Notes

###### Conventions for Naming Functions

* avoid generic names like 'data' or 'run'
* name your functions beginning with an action word (ie. createUser or sendUserData)
* be consistent with your naming conventions
* if you're joining an existing project, observe and adapt any existing conventions

Functions should focus on a single task: returning a value or causing a side effect. Break your function into additional smaller functions if you find it doing two or more things.

You should try to avoid using outer scope variables in your functions. Instead, pass in that data through parameters.