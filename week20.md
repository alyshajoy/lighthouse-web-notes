# Unit & Integration Testing

* tools for testing React
* coverage reports
* add features/tests to our app
* `debug()` and `prettyDOM()`
* mocking AJAX requests and functions

### Types of Testing

* static - linter, syntax highlighting
* manual - dev runs the code
* unit - testing the smallest pieces of our code (functions)
* integration - test larger pieces working together (ie. rendering components)
* end-to-end - test the app as a user would

### Jest

* is included in create-react-app (don't need to install it if you created your react app in this way)
* need to put all tests in a folder called "__tests__"
* a test suite is just a test file

**Tests:**
```js
describe('robotChoice function', () => {

  test('returns the winning choice if isCheating is true', () => {
    const playerSelection = 'Tree';
    const isCheating = true;

    const actual = robotChoice(playerSelection, isCheating);
    const expected = 'Axe';

    expect(actual).toBe(expected);
  });

  test('returns a valid choice when isCheating is false', () => {
    const playerSelection = 'Tree';
    const isCheating = false;

    const actual = robotChoice(playerSelection, isCheating);

    const options = ['Moai', 'Axe', 'Tree'];

    expect(options).toContain(actual);
  })
})
```
**Code to Make Tests Pass:**
```js
const robotChoice = (playerSelection, isCheating) => {
  if(isCheating) {
    const winningChoices = {
      Axe: 'Moai',
      Tree: 'Axe',
      Moai: 'Tree'
    }
    return winningChoices[playerSelection];
  } 

  const options = ['Moai', 'Axe', 'Tree'];
  const randomIndex = Math.floor(Math.random() * options.length);
  return options[index];
}
```

* npm run test -- --verbose
  * this tells jest to show you details about all tests (passed or failed)

```js
import {render, fireEvent} from '@testing-library/react';

describe('tests for the robot head icon', () => {

  test('can toggle the isCheating boolean by clicking on the robot head icon', () => {
    const { getByTestId } = render(<App />);

    const robotHeadIcon = getByTestId('robot-head-icon');

    fireEvent.click(robotHeadIcon); // virtually clicks the robot head

    expect(robotHeadIcon).toHaveClass('cheating');

    fireEvent.click(robotHeadIcon); // click head again to toggle it off

    expect(robotHeadIcon).not.toHaveClass('cheating');
  })

})
```

* render returns an object that returns a bunch of methods
  * query and get are synchronous
    * query won't fail if item isn't found (just returns null)
    * get causes test to fail if item isn't found
  * find is async

* fireEvent works asynchronously (and it waits for async event to happen before code continues to run) - don't have to use promises or anything

* you can add data-testid to DOM item you want to grab (this lets everyone on your team know that this is what you're using to test so they don't change it!)

* jest-dom:
  * provides custom matchers for us (ie. toHaveClass, toHaveFocus)

* you always want to test with hard-coded data
* axios is an object
  * so to mock it, create an object that duplicates what axios would be fetching
  * ie:

```js
const highScores = [
  {
    id: 1,
    name: 'Alice',
    score: 15
  },
  {
    id: 2,
    name: 'Bob',
    score: 10
  },
  {
    id: 3,
    name: 'Carol',
    score: 5
  }
];

const axios = {
  get: (url) => {
    if (url === '/high-scores') {
      return Promise.resolve({ data: highScores });
    }
  }
}
```

```js
test('can display the results from an API call', () => {
  // render our component
  const { getByTestId } = render(<Result status="Waiting" />);

  // find the high scores button
  const highScoresButton = getByTestId('high-scores');

  // click on the high scores button
  fireEvent.click(highScoresButton);

  // look in the DOM for one of our fake scores
  // because this is async, we need to use a 'find' method -> all find methods return promises
  return findByText('Bob' { exact: false }); // {exact: false} allows it to find something that isn't a perfectly exact match. Ie. 'Hello Bob' instead of 'Bob' 

})
```


# End-to-End Testing with Cypress

* what is Cypress?
* getting started with Cypress

### Testing Terms

* unit testing: testing individual pieces/"units" of our code. This usually means functions or components.
* integration testing: testing multiple units together
* manual testing: running our app (maybe adding some 'console.log') to see what works
* automated testing: running pre-written tests/assertions using a test-runner (like Mocha or Jest)
* end-to-end testing: "Big picture" - how does our app as a whole run from a user-like perspective?

**What is Jest?**
* automated
* ships with React
* is a testing framework
* intended for React and plays nicely with it
* CLI (command line interface) tool
* very fast
* integrated testing
* unit testing

**What is Cypress?**
* tool for end-to-end testing
* able to run a browser and incorporate this in its testing
  * allows us to script browser events to trigger things like typing into a field, clicking a link, pressing a button, etc.
* opens doors to responsive testing
* increases confidence in app behaviours
* can be used to test more than just basic code on/off, can see if the elements are visible in-page
* is capable of recording test sessions as videos
* is capable of taking screenshots of test case results
* completely agnostic to the stack or language you use to develop your app
* is very premium (GUI, extensive documentation) for a developer tool

"cypress": "./node_modules/.bin/cypress open" - include this in package.json to allow our app to run it easily

```js

describe('tests for checkboxes', () => {

  beforeEach(() => {
    cy.visit('http://localhost:3000'); // runs before each test to refresh page
  })

  it('can uncheck the "Explicit" checkbox', () => {
    
    // Targest element in page...
    cy.get('#Explicit')
    // Try to uncheck the element...
      .uncheck()
    // See if the element is checked or not.
      .should('not.be.checked');

  });

  it('can check the "Single" checkbox', () => {

    cy.get('#Single')
      .check()
      .should('be.checked')
  })

});
```

```js
describe('tests for the search input form field', () => {

  beforeEach(() => {
    cy.visit('http://localhost:3000');

    cy.get('[name="search"]')
      .as('searchInput'); // alias name we can use later
  })

  it('can type into the search input field', () => {

    const textValue = 'Born Ruffians';

    cy.get('@searchInput') // alias selector
      // try to type text into the search input field
      .type(textValue, { delay: 350 }) // delays each keystroke by 350ms
      // confirm that expected text is inside
      .should('have.value', textValue); // checks if value in textbox (have.value) is the same as textValue
  });

  it('can type and backspace into the search input field', () => {

    const textValue = 'Lester Young';
    const keystrokes = 'Lester Old{backspace}{backspace}{backspace}Young'; //{backspace} actually makes the test do a backspace

    cy.get('[name="search"]') // attribute selector (in real world would have used alias)
      .type(keystrokes, { delay: 350 })
      .should('have.value', textValue)
  });

});
```

Can use cy.visit('/') if we set up a base URL in config settings.
  * "baseUrl": "http://localhost:3000"
  * put this in cypress.json file

  **04-api.spec.js:**
  ```js
  it('can display API results after a search is typed', () => {

    // visit the web page
    cy.visit('/');

    // set-up our fixture (we use OUR test data, not the live API)
    // set up an interception (if the app calls to the search API, intercept and replace with our data)
    cy.intercept(
      'GET', // METHOD we're watching for
      '**/search*', // URL/PATH PATTERN (can find this info in inspet -> network tab) - '*' are wildcards, so this looks for any url that contains /search // HTTP/HTTPS(* for protocol) DOMAIN.COM(* for domain) /search ?anything=after(* for everything after)
      { fixture: 'itunes', delay: 8000 } // create fixtures file, with ___.json (itunes.json in this example) file that contains data in the identical format it would be from the real API // delay allows us to give the program time to load the results
    ).as('itunesResponse');

    // get the search input
    cy.get('[name="search"]')
      // type into the search input
      .type('Kendrick Lamar', { delay: 285 })

    // get the "loading spinner", make sure it displays while we are loading the results
    cy.get('.spinner')
      // assert that loading spinner is visible
      .should('be.visible');
    // wait until the results are received
    cy.wait('@itunesResponse'); // waits for the cy.intercept to be done

    // assert that a particular album does exist in our page (we're able to display results)
    cy.get(':nth-child(28) > .album__info > .album__name')
      .contains('Overly Dedicated')
      .should('be.visible');
  })
  ```