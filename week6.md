# CRUD with Express Lecture

### Express

* web app / web server framework
* a framework is something that has a bunch of built in objects, functions, etc. written for us that help us to get set up with a web server more quickly

### Routes

resource (our data)

GET /resource/new


GET /home:

```javascript
*`app.get('/home', (req, res) => {

})`

```

### CRUD

Create, Read, Update, Delete

### ICRUDES

* Index (show all)
* Create (show the "new" form)
* Read (show a single one of this resource)
* Update (submit the edit form) POST
* Delete (delete the resource) POST
* Edit (show the edit form)
* Save (submit the "new" form)

#### Index / Show All
GET /resource

#### Create
GET /resource/new -> SHOW the user the "new resource form"
POST /resource -> submit the form

#### Read
GET /resource/:id

#### Update
GET /resource/:id/edit -> SHOW the user the "edit resource form"
POST /resource/:id -> submit the edits

#### Delete
POST /resource/:id/delete

### Difference Between GET and POST:
* GET is a standard request - often retrieval
  * can send query parameters in our address in a GET (if needed)
  * is a repeatable action (can send someone else the URL address and they will be able to perform the exact same action)
  * easily shareable / bookmarkable

* POST is used to send information to the server via the body of the request
  * not easily repeatable
  * does not show in the address bar

<form method="GET" action="/search">
  <input type="text" name="q">
  https://www.google.com/search?q=Express+NodeJS
  (Express+NodeJS is what was entered into the form by the client)

## Data for Today

```js

const pets = {
  Kane: { name: 'Kane', type: 'Dog' },
  Quorra: { name: 'Quorra', type: 'Dog', age: 2 },
  Shadow: { name: 'Shadow', type: 'Rabbit', age: 5 },
  Booboo: { name: 'Booboo', type: 'Cat' },
  Ryder: { name: 'Ryder', type: 'Dog' },
  Mia: { name: 'Mia', type: 'Cat' },
  Keke: { name: 'Keke', type: 'Dog', age: 10 }
};

```

* I -> GET /pets   # Show all pets
* C -> GET /pets/new   # Show new pet form
* R -> GET /pets/:name   # Show info about this pet
* U -> POST /pets/:name   # Submission of edit form
* D -> POST /pets/:name/delete   # Submission of delete form
* E -> GET /pets/:name/edit   # Show edit pet form
* S -> POST /pets   # Submission of create/new form


## express-server.js

```js
// npm install express and ejs and morgan (morgan is a package that console.logs each time we get a request)

// remember to always create .gitignore any time we install an npm package to ignore node_modules

// Configuration:
const express = require('express');
const morgan = require('morgan');

const PORT = 8080;
const app = express();
app.set('view engine', 'ejs');

// Middleware:

app.use(morgan('dev')); // logs request data in our terminal
app.use(express.urlencoded({extended: true})); // parses url-encoded data in requests

// Listener
app.listen(PORT, () => {
  console.log(`Server is listening on ${PORT}!`);
});

// Database:
const pets = {
  Kane: { name: 'Kane', type: 'Dog' },
  Quorra: { name: 'Quorra', type: 'Dog', age: 2 },
  Shadow: { name: 'Shadow', type: 'Rabbit', age: 5 },
  Booboo: { name: 'Booboo', type: 'Cat' },
  Ryder: { name: 'Ryder', type: 'Dog' },
  Mia: { name: 'Mia', type: 'Cat' },
  Keke: { name: 'Keke', type: 'Dog', age: 10 }
};

// Routes

//* I -> GET /pets   # Show all pets
app.get('/pets', (req, res) => {
  const templateVars = { // templateVars should be an object
    pets: pets;
  };

  res.render('/pets/index', templateVars);
});

//* C -> GET /pets/new   # Show new pet form
app.get('/pets/new', (req, res) => {
  res.render('/pets/new');
});

//* R -> GET /pets/:name   # Show info about this pet
app.get('/pets/:name', (req, res) => {
  // access parameters (ie. :name) using req.params
  const name = req.params.name;
  const pet = pets[name];
  if(!pet) res.status(404).end('Pet not found'); // if pet name entered isn't in database, send a 404 error with the message "Pet not found"
  const templateVars = {
    pet: pet
  };
  res.render('/pets/read', templateVars)
});

//* U -> POST /pets/:name   # Submission of edit form

//* D -> POST /pets/:name/delete   # Submission of delete form
app.post('/pets/:name/delete', (req, res) => {
  const name = req.params.name;
  delete pets[name];
  res.redirect('/pets');
});

//* E -> GET /pets/:name/edit   # Show edit pet form

//* S -> POST /pets   # Submission of create/new form
app.post('/pets', (req, res) => {

  const name = req.body.name;
  const type = req.body.type;
  const age = parseInt(req.body.age); // form data is always sent as a string, so need to convert it to a number if that's what you want to happen

  const newPet = {
    name: name,
    type: type,
    age: age
  };

  pets[name] = newPet;

  res.redirect(`/pets/${name}`);
})


```