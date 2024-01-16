# Normalization in Database

* **database normalization** is the process of making the data in a database available in the most organized way possible
* two things you need to consider:
  1. Whether the information in the database has internal redundancies
  2. Whether the dependencies across the different tables are logically organized

* it is normal to create more tables when normalizing a database

**First Normal Form** - make sure that no table contains multiple columns that you could use to get the same information
* each table should be organized into rows, and each row should have a unique primary key
* each square (column, row) should only have one item

**Second Normal Form** - split out redundant rows into separate tables

**Third Normal Form** - make sure that each non-key element in each table provides information about the key in the row

**Fourth Normal Form** - 4NF is about separating information in a database to avoid duplication, and ensure that each piece of data is stored only once when there are multiple, independent relationships

CHAT GPT:

The Fourth Normal Form (4NF) in database normalization is about handling multi-valued dependencies, which are a bit complex but I'll try to simplify it.

Imagine you have a database table that includes information about students, their favorite subjects, and their hobbies. Some students have multiple favorite subjects and multiple hobbies. If you just put all this data into one table, you might have to repeat the student's name for each subject and each hobby. This creates unnecessary duplicates and can lead to inconsistencies.

4NF says that you should separate this information into different tables to avoid these problems. So, you'd have one table for students and their favorite subjects, and another table for students and their hobbies. By doing this, you ensure that each piece of information is stored only once, making the database more efficient and easier to manage.

### One-to-Many Relationships

1. What is a One-To-Many Relationship (OTMR)?
  * one record in one table is related to many records in another table
  * ie. customer order relationship: one customer can have many orders, but each order can only be related to one customer

2. Write a Sentence
  * helps us determine what type of relationship we are using
  * ie. "Store the customer information and the orders they have made.", "Store the cars and the showroom that they are located in.", "Store the students and the courses that they are enrolled in at a specific date."

3. Write Down the Objects from the Sentence
  * ie. the nouns
  * using above examples: "customers, orders", "cars, showroom", "students, courses, date"

4. Determine Type of Relationship
  * for OTMR, ask: "Does object1 have many object2s, or does object2 have many object1s?"

5. Create tables in an ERD

6. Draw lines between tables that have relationships.

7. Add to diagram to describe OTMR (ie. use crows foot notation)

**Foreign keys will be on the MANY side.**

In relational databases, we have no way of directly modeling a many-to-many relationship. Instead, we turn this into **two** one-to-many relationships using a join table.

Always design a database using an ERD before writing any code.

#### Naming Conventions

* use snake_case for table and column names
* pluralize tables names
* column names should be singular
* call your primary key "id"
* for *most* foreign keys, use *table*_id

# Lecture - SQL from our Apps

node-postgress is a collection of node.js modules for interfacing with your PostgreSQL database
* to install: npm install pg
* look at docs to learn how to use it

index.js:

```javascript
const pg = require('pg');

const Client = pg.Client; // single connection to the rdbms
// const Pool = pg.Pool; // collection of Clients (5); managed (means we don't need to worry about it)

const config = {
  host: 'localhost',
  port: 5432, // this is the default port, so unless you're changing the port you don't need to include this
  database: 'lightbnb',
  user: 'labber',
  password: 'labber'
};

const client = new Client(config);

client.connect();

client.query('SELECT * FROM movie_villains;') // if you don't give it a callback, it will give you back a promise
  .then((response) => {
    console.log(response)
    client.end() // severs connection once you get the information you wanted
  })
  .catch((response) => {
    console.log(response)
  });
```

Result {
  rows: [
    {id: 1, villain: 'Agent Smith', movie: 'The Matrix'},
    {id: 2, villain: 'Voldemort', movie: 'Harry Potter'},
    {id: 3, villain: 'Wicked Witch', movie: 'Wizard of Oz'},
    {id: 4, villan: 'Thanos', movie: 'Infinity War'}
  ]
}

client <===tcp/http===> web server <===tcp/postgres===> rdbms

response.rows is always an array (no matter how many rows you are returning)

In SQL, arrays start at 1, not 0.

To prevent SQL injection attacks: client.query('SELECT * FROM movie_villains WHERE id = $1;', [id])