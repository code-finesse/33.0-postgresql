# 33.0-postgresql

# SQL Databases

## Relational Databases

- has the ability to relate one value/collection/document to another (in MongoDB we use refs or directly embed schemas into other schemas)
- data is stored using the *relational model* in mathematics
- the way data is organized is fundamentally different in a RBD than in a non-relational DB (also called noSQL sometimes

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27aaabea-e813-4667-abfd-f28645afe73e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27aaabea-e813-4667-abfd-f28645afe73e/Untitled.png)

- Terminology

    **Database:** The actual set of data being stored

    - We may have multiple databases for an application

    **Database Language:** The language used to interact with a database

    - With relational databases, we use SQL
    - There isn't a standard language across noSQL databases

    **Database Management System (DBMS):** The software that lets a user interact (query) the data in a database

    - Relational examples include PostgreSQL and MySQL
    - Nonrelational include MongoDB, Redis, CouchDB

    **Database CLI:** A tool offered by most DBMSs that allow users to query the database from the command line

    - We will use one called `psql` for PostgreSQL
    - MongoDB had the mongo shell
- SQL vs NoSQL
    - TLDR

        ## **Relational vs Non-Relational | PostgreSQL vs MongoDB**

        Non-Relational or **noSQL** databases have existed in some form for decades, however their use didn't become wide spread until recently. noSQL databases became an important alternative to relational databases in the early 2000s as internet tech companies' data storage needs changed and expanded. With the rise of social media and online marketplaces like eBay, the amount of data on the internet boomed. Users were not only getting information from the internet, they were contributing to it. This transition stressed the capabilities of relational databases due to the volume and variability of user-generated data.

        noSQL databases **generally** offer more flexibility and scalability than traditional relational databases. However, they come with the cost of reduced consistency.

        ### **MongoDB is non-relational (noSQL)**

        MongoDB is document-based. Meaning, data is organized in collections of related documents formatted in JSON.

        ### **Key Advantages of NoSQL**

        ### **Usability**

        - Documents (i.e. JSON objects) correspond to native data types in many programming languages (like mongo + JSON)
        - Documents can contain data that varies or is incomplete, no need for migrations

        ### **High Performance**

        - Documents can be embedded in one another reducing the need for joins.
        - Simple queries are very fast

        ### **PostgreSQL is relational (SQL)**

        PostgreSQL is a relational database management system. There are many others like MySQL, MS SQL, Oracle, and sqlite. They are all queried using SQL.

        ### **Key Advantages of SQL**

        ### **Consistency**

        - A lot of data is tabular already, relational databases store it in a similar form
        - Schemas mean you know exactly what attributes (columns) for each database entry (row)
        - Schemas can check the type of data being stored to ensure data coming in is properly formatted and consistent with other entries.
        - Writes to a database follow ACID paradigm (atomicity, consistency, isolation, durability). More about [ACID](https://en.wikipedia.org/wiki/ACID)

        ### **SQL Dialect**

        - SQL is used in most relational databases meaning interaction across different relational DBMS is very similar
        - SQL is well documented and extremely robust in its utility
        - SQL queries are a powerful tool to quickly retrieve data in a large variety of ways.

        ### **Both offer:**

        ### **Automatic Scaling**

        - Sharding distributes data across a cluster of servers (data can be broken up across different remote hosts)
        - Replica sets provide low-latency high-throughput deployments (multiple data sets provide redundancy and allow for rapid updating)

    [SQL vs NoSQL](https://www.notion.so/e9b40d7d99964f3eb4987762627932cd)

### Postgres CLI

- CRUD commands

    ```jsx
    help -- general help
    \?   -- help with psql commands
    \h   -- help with SQL commands
    \l   -- Lists all databases
    \q   -- exits psql
    q    -- exits a psql list or dialogue

    CREATE DATABASE generalassembly; -- Don't forget the semicolon!
    \l -- What changed?

    \c generalassembly -- Connect to generalassembly database

    \d -- Lists all tables

    CREATE TABLE students (
      id SERIAL PRIMARY KEY,
      first_name VARCHAR NOT NULL,
      last_name VARCHAR NOT NULL,
      quote TEXT,
      birthday VARCHAR,
      ssn INT NOT NULL UNIQUE
    ); -- Here we are defining a schema. More on this in just a bit...

    \d

    SELECT * FROM students;

    INSERT INTO students (first_name, last_name) VALUES ('Andy', 'Whitely');
    -- This won't work!

    INSERT INTO students (first_name, last_name, quote, birthday, ssn) VALUES ('Andy', 'Whitely', 'Two goldfish are in a tank. One says, "Know how to drive this thing?"', 'April 7', 8675309);
    SELECT * FROM students;

    UPDATE students SET first_name = 'Andrew' WHERE first_name = 'Andy';
    SELECT * FROM students;

    DELETE FROM students WHERE first_name = 'Andy';
    DELETE FROM students WHERE first_name = 'Andrew';

    SELECT * FROM students;

    DROP TABLE students; -- "drop" means to delete an entire table or database

    \d

    DROP DATABASE generalassembly;
    // you need to be outside of a database in order to drop it

    \q --quit
    ```

    - backslash commands are commands to navigate psql and are psql-specific
    - everything else is SQL. the SQL  is what actually runs commands and interacts with the database
    - it is possible to write multiple lines in the terminal
    - commands need to be terminated with the ;(semicolon)
- Cheat Sheet

    [PostgreSQL-Cheat-Sheet.pdf](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/af731189-5ae4-4ebd-8eae-8e1f96581cf2/PostgreSQL-Cheat-Sheet.pdf)

### SQL Syntax

- whitespace doesn't matter (unless it splits up a word)
- Case sensitive for values (tables, columns, etc), but not commands (select, insert, drop).
- Always use single quotes when typing out string values

```jsx
-- gets all values from a table
SELECT * FROM table_name;

-- gets two columns from a table
SELECT column_name, other_column FROM table_name;

-- gets two columns from a table if a certain critera is true
SELECT column_name, other_column FROM table_name WHERE some_value > 100;

-- gets two columns from a table if a certain critera is true, only returning 10 records, in descending order
SELECT column_name, other_column FROM table_name WHERE value > 100 LIMIT 10 ORDER BY DESC ;
```

- Schema

    Every application's database will have one or more tables. You will recall, each table stores information about a particular model (e.g., `artists`, `songs`).

    Each table has a **schema**, which defines and enforces what columns it has. For each column the schema defines...

    - The column's name
    - the column's data type
    - Any constraints for that column

    ### **Common Data Types**

    Here are some common data types for SQL databases. They are all, for the most part, things you've seen before...

    - boolean
    - integer
    - float
    - text / VARCHAR
        - VARCHAR can be of variable length, TEXT is unlimited
    - date
    - time

    > And many more...

    ### **Constraints**

    Constraints act as limits on the data that can go in a column.

    - e.g., `NOT NULL` and `UNIQUE`

    > And many more...

    ### **Defining a Schema**

    Next we're going to build a schema for a database in a sample application. It can change later on if we need to add / remove tables or columns, but we'll start with something simple.

    Instead of typing this into `psql`, you can write to a `.sql` file and run it, just like we have with `.js` and `.rb` files.

    ## 

### Relationships

*need to add more info here

### Joins

- to `SELECT` information on two or more tables at ones, we can use a `JOIN` clause
- joins will produce rows that contain information from both tables
- when joining two or more tables, we have to tell the database how to match up the rows. (e.g. to make sure the author information is correct for each book).
- joins use the `ON` clause, which specifies which properties to match.

```jsx
SELECT id FROM authors where name = 'J.K. Rowling';
SELECT * FROM books where author_id = 2;

SELECT * FROM books JOIN authors ON books.author_id = authors.id;
SELECT * FROM books JOIN authors ON books.author_id = authors.id WHERE authors.nationality = 'United States of America';
```

[https://blog.codinghorror.com/a-visual-explanation-of-sql-joins/](https://blog.codinghorror.com/a-visual-explanation-of-sql-joins/)

### Executing a File

There are two ways to execute a sql file using psql.

- Using your normal shell (be sure you have changed directory into this repo):

```jsx
$ psql -d library < schema.sql
```

- Or from inside the psql cli:

```jsx
-- first cd to the directory where the file is
-- then launch the psql cli and connect to the right db
$ psql -d library
-- execute using \i and the filename
\i schema.sql
```

- databases can be seeded with .sql files
- .sql files can also be run to provide information through the terminal
- the most common SQL commands (INSERT, SELECT, UPDATE, AND DELETE) can be run from the cli or the .sql file

### MIN, MAX, COUNT, OTHER RANDOM ISH

- parameters can be added to the CRUD commands to specify specific pieces of information
- NFL Examples

    ```jsx
    1. List the names of all NFL teams
    -- SELECT name FROM teams;

    2. List the stadium name and head coach of all NFC teams
    -- SELECT stadium, head_coach FROM teams WHERE conference = 'NFC';

    3. List the head coaches of the AFC South
    -- SELECT head_coach FROM teams WHERE conference = 'AFC' AND division = 'South';

    4. The total number of players in the NFL
    -- SELECT COUNT(*) FROM players;

    5. The team names and head coaches of the NFC North and AFC East
    -- SELECT name, head_coach FROM teams WHERE conference = 'NFC' AND division = 'North';
    -- SELECT name, head_coach FROM teams WHERE conference = 'AFC' AND division = 'East';

    6. The 50 players with the highest salaries
    -- SELECT * FROM players ORDER BY salary DESC LIMIT 50;

    7. The average salary of all NFL players
    -- SELECT AVG(salary) FROM players;

    8. The names and positions of players with a salary above 10_000_000
    -- SELECT name, position FROM players WHERE salary > 10000000;

    9. The player with the highest salary in the NFL
    -- SELECT * FROM players ORDER BY salary DESC LIMIT 1;

    10. The name and position of the first 100 players with the lowest salaries
    -- SELECT name, position FROM players ORDER BY salary ASC LIMIT 100;

    11. The average salary for a DE in the nfl
    -- SELECT AVG(salary) FROM players WHERE position = 'DE'; 

    12. The names of all the players on the Buffalo Bills
    -- SELECT * FROM players JOIN teams ON players.team_id = teams.id WHERE teams.name = 'Buffalo Bills';
    -- SELECT players.name, teams.name FROM players, teams WHERE players.team_id=teams.id AND teams.name LIKE 'Buffalo Bills';

    13. The total salary of all players on the New York Giants
    -- SELECT SUM(salary) FROM players JOIN teams ON players.team_id = teams.id WHERE teams.name = 'New York Giants';

    14. The player with the lowest salary on the Green Bay Packers
    -- SELECT * FROM players JOIN teams ON players.team_id = teams.id WHERE teams.name = 'Green Bay Packers' ORDER BY salary ASC LIMIT 1;
    ```
