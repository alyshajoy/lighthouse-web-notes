# Lecture: TDD with Mocha and Chai

When testing, we want to call the function with known arguments. We expect to get back a known/predictable result.
  * actual vs expected (tests if they are equal)


### Node

  * a runtime (the environment in which the code runs) for JS
    * a browser is an example of another runtime
  * what runs our code

  * node has its own assertion library
  * to bring it in => const assert = require("assert");
    * you can console.log(assert) to see the contents of assert.. it shows you all the functions available through node assert

  * in node, a module is a file
  * with require(), if you start it with a . (ie. require("./index)), node knows to look within your own code
  * you don't need to include the file extension

  * if you want to export more than one thing, you can do so by adding key/value pairs to the module.exports object

  * node has a wrapper function
    * before each file's code is executed, note wraps it in a function
    * the module code actually lives in here
    * module isn't actually in javascript


### npm

* npm doesn't actually stand for anything
* npm is a collection of packages of other people's code
* yarn is another collection of packages that you can install (not used in this program though)
* in order to install other packages, we need to install our own package
  * done though "npm init"
  * all npm init does is creates a package.json file
* always do npm init before installing other packages
* npm is like the Amazon of software

* package-lock file locks in the versions of the things we have downloaded - that way, if there are multiple people working on the project, it ensures they are all using the same versions of the installed packages

* dependencies are packages that need to be downloaded for our code to run
* devDependencies are packages that are needed for the developers to do everything we want the code to do (ie. this includes testing, which only the developers use - tests aren't included in the actual code once its finished)
* when you install a package, npm will look at all the dependencies of the package you want, and it will install all of those packages as well

### Mocha

* you must create a directory called "test" (cannot name it anything else)
  * put all your test files in here (can name these whatever you want)

Mocha provides two functions:

1. describe("description here", () => {put it tests into here}) 
  * always optional
  * use this to group tests together
2. it("description of what we expect the code to do", () => {test code goes into here})
  * it is a test
  * each test will be in its own it function

* tests will pass unless there is an error

* "assert" function comes from node
* you can write as many assertions as you want per test


### Test Driven Development

* tests are written BEFORE the code is written
* n-Driven Development

* npm install installs the dependencies in the json package file

### Git Ignore

* .gitignore
  * is a hidden file
* add the name of any file or directory that we want git to ignore
* we typically add node_modules to gitignore
* set up gitignore when you first create your project