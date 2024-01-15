# SQL Intro

### Database
* collection of tables
* a table is a grouping of rows and columns

### SQL
* stands for Structured Query Language

* rows => records
* columns => fields

### Relational Databases
* each table is related to one or more other tables

### Primary Key
* unique identifier
* usually numbers autoincrementing

|ID|Food Item|Price|Calories|
|---|---|---|---|
|1|Big Mac|4.99|600|
|2|Large Fries|2.99|400|

* foreign key => one table's primary key stored in another table

### RDBMS
* Relational DataBase Management System/Server
* every query we ask of our database runs through the RDBMS
* we can't connect directly to a database - instead, we always go through an RDBMS

client <===tcp/http====> server <===tcp/postgres(or mysql, etc)===> rdbms

This is the full stand: front end, back end, database

* front end cannot talk to database -> front end talks to back end, which then talks to the database

### SELECT Challenges

Everything we write is known as a query.

SQL is imperative. NOT declarative.

We write what we want to get accomplished, not how it will get accomplished.

Each table's name should be plural and lowercase.

SQL is case insensitive, so we typically use snake-case.
* it also doesn't care at all about line breaks or white space
* select * from users is the exact same as SelECt * FROm usERs
* industry standard is the put table names in lowercase, and action works in all caps
  * SELECT * FROM users;

Elephant SQL can run your RDBMS for you in the cloud.

Best practice is to specifically name the fields you are interested in when writing your queries. (ie. don't use SELECT *)

SQL errors generally give you almost no information.

1. List total number of users.
```sql
SELECT COUNT(id) AS num_users 
FROM users;
```
COUNT() is a built in function that counts how many of something you have inside that column.

AS gives an alias name to the metadata result.

Result is:
num_users 20000

2. List all users over the age of 18.
```sql
SELECT *
FROM users
WHERE age > 18;
```
WHERE is a keyword that filters infomation

For checking equality, use = and NOT ===

3. List users who are over the age of 18 and have the last name 'Barrows'
```sql
SELECT *
FROM users
WHERE age > 18 AND last_name = 'Barrows';
```
You cannot have more than one WHERE clause.

Instead, you list all filters in one WHERE clause, using the words AND or OR

Double quotes are used to identify columns or tables, so if looking for a string, you must use single quotes.

4. List users over the age of 18, who live in Canada, sorted by age from oldest to youngest, and then last name alphabetically.
```sql
SELECT *
FROM users
WHERE age > 18 AND country = 'Canada'
ORDER BY age DESC, last_name;
```
ORDER BY sorts the returned data. You can sort by ascending (ASC) or descending (DESC). If you don't put ASC or DESC, ascending is the default.

When sorting in multiple ways, use commas to separate sorts (not AND keyword).

The secondary (or tertiary) sorts will only be implemented if there is a tiebreaker in the first sort (ie. there is more than one 21 year old, so then sort all 21 year olds alphabetically).

5. List users who live in Canada and whose accounts are overdue.
```sql
SELECT *
FROM users
WHERE country = 'Canada' AND payment_due_date < 'January 9, 2024';
```
'Jan 9, 2024' would also work, as well as '01/09/24' (although you would need to ensure its month/day/year unless you manually change it in your RDBMS settings)

If we don't want the date hardcoded:
```sql
SELECT *
FROM users
WHERE country = 'Canada' AND payment_due_date < NOW();
```
NOW() gives that exact current date and time.

If we just want the date (and not the exact time):
```sql
SELECT *
FROM users
WHERE country = 'Canada' AND payment_due_date < CURRENT_DATE;

-- use double dash for comments
```

6. List all albums along with their songs.
```sql
SELECT *
FROM albums
JOIN songs ON albums.id = songs.album_id;
```
You could also say songs.album_id = albums.id (doesn't matter the order).

You could also just say album_id (dont need to include songs.) because album_id only exists in one table.

This takes the two tables and returns one table with the data combined.

```sql
SELECT *
FROM songs
JOIN albums ON albums.id = album_id;
```
Returns the exact same information as the first table, but the columns are just in different orders. Typically, you can list your ones first and connect it to the many.

7. List all albums along with how many songs each album has.
```sql
SELECT album_name, COUNT (songs.*) AS num_songs
FROM songs
JOIN albums ON albums.id = songs.album_id
GROUP BY album_name;
```
GROUP BY is how you group like with like.

8. Enhance the previous query to only include albums that have more than 10 songs.
```sql
SELECT album_name, COUNT(songs.*) AS num_songs
FROM songs
JOIN albums ON albums.id = songs.album_id
GROUP BY album_name
HAVING COUNT(songs.*) > 10;
```
SELECT is just what information we get back.

WHERE asks direct questions about the table.

HAVING is used when we want to filter by aggregated data.

Order that we need to put our queries in:
1. SELECT
2. FROM
3. JOIN
4. ON
5. WHERE
6. GROUP BY
7. HAVING
8. ORDER BY
9. LIMIT

Order it actually runs in:
1. FROM and JOIN
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. DISTINCT
7. ORDER BY
8. LIMIT / OFFSET

9. List ALL albums along with their songs
```sql
SELECT *
FROM albums
LEFT JOIN songs ON albums.id = songs.album_id;

-- this is a comment
```
INNER JOIN produces only the set of records that match in both Table A and Table B. This is the default.

Left (outer) join produces a complete set of records from Table A, with the matching records (where available) in Table B. If there is no match, the right side will contain null. The inverse is true for a Right Join.

```sql
SELECT * FROM TableA A
LEFT JOIN Table B B ON
A.key = B.key;
```

FULL OUTER JOIN produce the set of all records in Table A and Table B.

SQL Joins Visualizer website helps clarify this!

10. List ONLY the first 10 songs from the songs table (LIMIT and OFFSET)
```sql
SELECT *
FROM songs
LIMIT 10 OFFSET 10;
```
LIMIT is always the last thing that fires.

OFFSET skips data. So in the example above, it would show datapoints 11-20.

To use pagination:

OFFSET page_number - 1 * amount_per_page;

Aggregation functions: AVG(), MIN(), MAX(), SUM(), COUNT()


# DATABASE DESIGN

Databases are often the most important piece of an application (ie. emails, facebook, twitter, photo storage, patient records, etc.).

### What is a relational database made up of?

* Tables / Entities / Nouns
* Each table has...
  * columns / fields
  * rows / records

### Common Naming Conventions

* changes depending on which database management system you use

We will learn the conventions for PostgreSQL.
* use snake case
* match column names with table names (when columns are using foreign keys from other tables)

Many to One:
* where something from one column can be related to many items of another table

One to One:
* always a direct one to one
* both tables can only have one possible connection


### Data Types

* https://www.postgresql.org/docs/current/datatype.html -> docs for postgres data types
* there are way more than the ones we'll go over today

Examples of Data Types:
* integer
* varchar (variable-length with limit)