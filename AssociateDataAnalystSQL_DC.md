# Career Track: Associate Data Analyst in SQL 

---

## Índice

- [1. Introduction to SQL](#1-introduction-to-sql)
  - [1.1 Relational Databases](#11-relational-databases)
    - [1.1.1 Databases](#111-databases)
    - [1.1.2 Tables](#112-tables)
    - [1.1.3 Data Types](#113-data-types)
  - [1.2 Consultas](#12-consultas)
    - [1.2.1 introducing querie](#121-introducing-querie)
    - [1.2.2 Writing Queries](#122-writing-queries)
    - [1.2.3 SQL Flavors](#123-sql-flavors)

---

- [2. SQL Intermedio Intermedio](#2-sql-intermedio-intermedio)
  - [2.1 Selecting Data](#21-selecting-data)
    - [2.1.1 Querying a database](#211-querying-a-database)
    - [2.1.2 Query Execution](#212-query-execution)
    - [2.1.3 SQL Style](#213-sql-style)
  - [2.2 Filtering Records](#22-filtering-records)
  - [2.3 Aggregate Functions](#23-aggregate-functions)
  - [2.4 Sorting and Grouping](#24-sorting-and-grouping)
  - [PROJECT 1: Analyzing Students' Mental Health](#project-1-analyzing-students-mental-health)

---

- [3. Joining Data in SQL](#3-joining-data-in-sql)
  - [3.1 Introducing Inner Joins](#31-introducing-inner-joins)
  - [3.2 Outer Joins, Cross Joins and Self Joins](#32-outer-joins-cross-joins-and-self-joins)
  - [3.3 Set Theory for SQL Joins](#33-set-theory-for-sql-joins)
  - [3.4 Subqueries](#34-subqueries)

---

- [4. Data Manipulation in SQL](#4-data-manipulation-in-sql)
  - [4.1 We'll take the CASE](#41-well-take-the-case)
  - [4.2 Short and Simple Queries](#42-short-and-simple-queries)
  - [4.3 Correlated Queries, Nested Queries, and Common Table Expressions](#43-correlated-queries-nested-queries-and-common-table-expressions)
  - [4.4 Window Functions](#44-window-functions)

---

- [5. PostgreSQL Summary Stats and Window Functions](#5-postgresql-summary-stats-and-window-functions)
  - [5.1 Introduction to window functions](#51-introduction-to-window-functions)
  - [5.2 Fatching, Ranking, and Paging](#52-fatching-ranking-and-paging)
  - [5.3 Aggregate window functions and frames](#53-aggregate-window-functions-and-frames)
  - [5.4 Beyond window functions](#54-beyond-window-functions)

---

- [6. Functions for Manipulting Data in PostgreSQL](#6-functions-for-manipulting-data-in-postgresql)
  - [6.1 Overview of Common Data Types](#61-overview-of-common-data-types)
  - [6.2 Working with DATE/TIME Functions and Operations](#62-working-with-datetime-functions-and-operations)
  - [6.3 Parsing and Manipulating Text](#63-parsing-and-manipulating-text)
  - [6.4 Full-text Search and PostgresSQL Extensions](#64-full-text-search-and-postgresql-extensions)

---

- [7. Introduction ton Statistics](#7-introduction-ton-statistics)
  - [7.1 Summary Statistics](#71-summary-statistics)
  - [7.2 Probability and Distributions](#72-probability-and-distributions)
  - [7.3 More Distributions and the Central Limit Theorem](#73-more-distributions-and-the-central-limit-theorem)
  - [7.4 Correlation and Hypothesis Testing](#74-correlation-and-hypothesis-testing)

---

- [8. Exploratory Data Analysis (EDA) in SQL](#8-exploratory-data-analysis-eda-in-sql)
  - [8.1 What's in the Database?](#81-whats-in-the-database)
  - [8.2 Summarizing and Aggregating Numeric Data](#82-summarizing-and-aggregating-numeric-data)
  - [8.3 Exploring Categorical DAta and Unstructured Text](#83-exploring-categorical-data-and-unstructured-text)
  - [8.4 Working with Dates and Timestamps](#84-working-with-dates-and-timestamps)

---

- [9. Data-Driven Decision Making in SQL](#9-data-driven-decision-making-in-sql)
  - [9.1 Introduction to Business Intelligence for a Online Movie Rental Database](#91-introduction-to-business-intelligence-for-a-online-movie-rental-database)
  - [9.2 Decision Making with Simple SQL Queries](#92-decision-making-with-simple-sql-queries)
  - [9.3 Data Driven Decision Making with Advanced SQL Queries](#93-data-driven-decision-making-with-advanced-sql-queries)
  - [9.4 Data Driven Decision Making with OLAP SQL Queries](#94-data-driven-decision-making-with-olap-sql-queries)

---

- [10. Understanding Data Visualization](#10-understanding-data-visualization)
  - [10.1 Visualizing Distributions](#101-visualizing-distributions)
  - [10.2 Visualizing Two Variables](#102-visualizing-two-variables)
  - [10.3 The Color and The Shape](#103-the-color-and-the-shape)
  - [10.4 99 Problems but a Plot ain't one of them](#104-99-problems-but-a-plot-aint-one-of-them)

---

- [11. Data Communication Concepts](#11-data-communication-concepts)
  - [11.1 Storytelling with Data](#111-storytelling-with-data)
  - [11.2 Preparing to Communicate the Data](#112-preparing-to-communicate-the-data)
  - [11.3 Structuring Written Reports](#113-structuring-written-reports)
  - [11.4 Building Compelling Oral Presentations](#114-building-compelling-oral-presentations)

---
---

## Disclaimer
This document contains only my personal notes from the courses I have taken on DataCamp. Most of the examples included are based primarily on those presented in the respective courses, and therefore, full credit is given to DataCamp and its instructors for the original content and instructional materials. These notes have been compiled strictly for my own personal reference and learning purposes. They are not intended for any commercial use or to generate any form of income. The sole purpose of this document is to serve as a study aid and quick reference for my own educational development.

---
---

## 1. Introduction to SQL 

### 1.1 Relational Databases

#### 1.1.1 Databases

- SQL. SQL, or Structured Query Language, is the most widely used programming language for communicating with data in databases.
- It lets us quickly access, organize, and analyze large amounts of data with direct commands, known as **queries**.
- A **table** is a component of a database. 
- Tables organize data into rows and columns. Rows contain individual data, and each column describes a specific part of that data.
- Several tables usually make up a database, and they work together through relationships.
- **Relational databases** include tables that share information. This creates connections between them. 
- When a database is queried, the data stored inside the database does not change; rather, the database information is accessed and presented according to instructions in the query.

#### 1.1.2 Tables

- Tables names should be in lower case and use underscores instead of spaces.
- Databases are organized into tables, which are organized into rows and columns.
- Rows = **Records** -> contains the data for an individual observation
- Columns = **Fields** -> contains one piece of information for every record in a table.
- *Field names* must be typed out when a querying a database with SQL, then:
  * Should be lowercase;
  * Use underscores intead of spaces;
  * Should be singular;
  * Fields in a table cannot have the same name
  * They should never share a name
- **Unique Identifiers** or **Key Values** must be unique for every record.
- Using several, well-organized tables is often better than putting everything into one big table.
- Keeping data in separate, related tables and connecting them with SQL, we can analyze and answer questions more easily than we could with spreadsheets.

El siguiente código nos permite explorar qué datos contiene `books`:
```sql
SELECT *
FROM books;
```

#### 1.1.3 Data Types

- Data is stored on a server's hard disk.
- **Data Type** depends on the specific information stored in the field, such as numbers, text or dates.
- We also want to consider the **type of operations** that we want to apply to that information.
  * **Strings** -> is a sequence of characters such as letters, numbers or punctuation. SQL has several different data types that can hold strings.
    - **VARCHAR** data type is commonly used due to its flexibility to store small or large strings.
  * **Integers** -> store whole numbers
    - **INT** is a common SQL integer data type that can store numbers from less than negative two billion to more than positive two billion.
  * **Floats** -> store numbers that include a fractional part or a decimal point.
    - **NUMERIC** can store floats with up to 38 digits total, including those before and after the decimal point.

- **Database Schema:**
  * Are often referred to as "blueprints" of databases.
  * Shows a database's design, such as what tables are included, any relationships between its tables, and what data type each field can hold.

### 1.2 Consultas

#### 1.2.1 introducing querie

- **Keywords** are reverved words used to indicate what operation we would like our code to perform.
  * **SELECT** -> indicates which fields (columns) should be selected. 
  * **FROM** -> indicates the table in which these fields (columns) are located.
 
- If we put these parts together... we are writing a query, and we end the query with a semicolon to indicate that the query is complete:
```SQL
SELECT var_name (column's name)
FROM table_name;
```
- Remember that keywords are capitalized while keeping table and field names all lowercase.
- **Result Set** -> the results of our query.
- To select multiple fields, we can list multiple field name after the `SELECT` keyword, separated by commas. For example:
```SQL
SELECT var1_name, var2_name
FROM table_name;
```
- We can tell SQL to select all fields using an asterisk or star `*`, also known as a **wildcar character**.
```SQL
SELECT *
FROM table_name;
```

#### 1.2.2 Writing Queries

- If we need to rename columns in our result set, whether for clarity of brevity, we can do this using **aliasing**.
- Then, for example we can use de `AS` keyword to alias one variable's name:
```SQL
SELECT var_name AS new_var_name
FROM results_table_name;
```
- Some SQL questions requiere a way yo return a list of unique values. Then we can use de `DISTINCT` keyword after the `SELECT` statement.
```SQL
SELECT DISTINCT var_name
FROM results_table_name;
```
- Also it is possible to return the unique combinations of multiple *field* values by listing multiple fields after the `DISTINCT` keyword. For example:
```SQL
SELECT DISTINCT var1_name, var2_name
FROM results_table_name;
```
- **View** -> is a saved SQL query that acts like a virtual table. Meaning that SQL views don't store data; they store the query, ensuring the results are always up-to-date with the latest database changes.
- To create a **VIEW** we use:
```SQL
CREATE VIEW view_name AS
```
- Creating a `VIEW` does not produce a result set, it only saves the query for reuse.
- Once a query is created we can query just as we would a *normal table* by selecting `FROM` the view.

#### 1.2.3 SQL Flavors
- SQL has several different versions or flavors, ranging from free versions to those designed for major databases like Microsoft SQL Server or Oracle Database.
- **PostgreSQL** is a free and open-source relational database system.
- **SQL Server** is a relational database system available in both free and enterprise versions. It was created by Microsoft.
- While there are many similarities between SQL Server and PostgreSQL, there are some small differences worth noting.
- For example, if we want to limit the number of employee names and IDs selected to only the first two records, PostgreSQL uses the LIMIT keyword. In contrast, SQL Server achieves the same result using the TOP keyword.
- If we want to use PostgreSQL to select the `genre` field from the `books` table and limit the number of results to 10, we do:
```SQL
SELECT genre
FROM books
LIMIT 10;
```
- If instead we would like to use SQL Server we do:
```SQL
SELECT genre TOP(10)
FROM books;
```
---

## 2. SQL Intermedio Intermedio

### 2.1 Selecting Data

#### 2.1.1 Querying a database

- A query is a request for data from a database.
- In this course, we worked with a films database containing four tables:
 * Films
 * Reviews
 * People
 * Roles

- `COUNT` -> function that lets us count... returning the number of records with a value in a field. For example:
```SQL
SELECT COUNT(birthdate) AS count_birthdates
FROM people;
```

- If we want to count more than one field, we need to use `COUNT` multiple times. For example:
```SQL
SELECT COUNT(name) AS count_names, COUNT(birthdate) AS count_birthdates
FROM people;
```

- If we want to count the number of records in a table, we can call `COUNT` with an asterisk. Then, passing the asterisk to `COUNT` is a shortcut for counting the total nubmber of records.
```SQL
SELECT COUNT(*) AS total_records
FROM people;
```

- Often, our results will include duplicates. Then, we can use the `DISTINCT` keyword to select all the unique values from a field. Adding `DISTINCT` to our query will remove all duplicates.
```SQL
SELECT DISTINCT language
FROM films;
```

- Combine `COUNT()` with `DISTINCT` is also common to count the number of unique values in a field. For example:
```SQL
SELECT COUNT(DISTINCT birthdate) AS count_distinct_birthdates
FROM people;
```
- `COUNT` would include all the duplicates while `DISTINCT`counts all of the unique dates, no matter how many times they come up. 

-  If you want to count the number of non-missing values in a particular field, you can call `COUNT()` on just that field.
- Other examples:
  * Count the number of records in the people table:
```SQL
SELECT COUNT(*) AS count_records
FROM people;
```
  * Count the number of birthdates in the people table:
```SQL
SELECT COUNT(birthdate) AS count_birthdate
FROM people;
```
  * Count the records for languages and countries represented in the films table:
```SQL
SELECT COUNT(language) AS count_languages, COUNT(country) AS count_countries
FROM films; 
```
  * Return the unique countries from the films table:
```SQL
SELECT DISTINCT country
FROM films;
```
  * Count the distinct countries from the films table:
```SQL
SELECT COUNT(DISTINCT country) AS count_distinct_countries
FROM films;
```

#### 2.1.2 Query Execution

- Unlike many programming languages, SQL code is not processed in the order it is written.
- Knowing processing order is especially useful when debugging and aliasing fields and tables.
- Some error messages are extremely helpful, pinpointing and even suggesting a solution for the error.
- Other error messages are less helpful and require us to review our code more closely.
- For example: forgetting a comma is a very common error. In this case, the error message will alert us to the general location of the error using a caret below the line of code, which in this case points to the "country" field name. We must examine the code a little further, though, to discover the missing comma.

```SQL
SELECT nme
FROM people;
```

```
field "nme" does not exist
LINE 1: SELECT nme
               ^
HINT: Perhaps you meant to reference the field "people.name".
```

- SQL displays a similar error message when a keyword is misspelled, but this time, the caret indicator below the offending line is spot on.

```SQL
SELECT title, country, duration
FROM films;
```

```
syntax error at or near "SELECT"
LINE 1: SELECT title, country, duration
        ^
```

- Some examples:
  * Debug the following code:
```SQL
SELECT certfication
FROM films
LIMIT 5;
```

If you run the previous code, you get the following error message: 
```
column "certfication" does not exist
LINE 2: SELECT certfication
               ^
HINT:  Perhaps you meant to reference the column "films.certification".
```

Then, after correcting the error the final code is:
```SQL
SELECT certification
FROM films
LIMIT 5;
```

  * Debug the following code:
```SQL
SELECT film_id imdb_score num_votes
FROM reviews;
```

If you run the previous code, you get the following error message: 
```
syntax error at or near "num_votes"
LINE 1: SELECT film_id imdb_score num_votes
                                  ^
```

Then,  after correcting the error the final code is:
```SQL
SELECT film_id, imdb_score, num_votes
FROM reviews;
```

  * Debug the following code:
```SQL
SELECT COUNNT(birthdate) AS count_birthdays
FROM peeple;
```

If you run the previous code, you get the following error message: 
```
relation "peeple" does not exist
LINE 2: FROM peeple;
             ^
```

What is the first issue?

The error is due to a typo in the table name. The query is trying to select data from a table named "peeple," but there is no such table in the database. The correct table name is "people". 

Then, if you run again this corrected version of the code, you get a new error message: 
```
function counnt(date) does not exist
LINE 2: SELECT COUNNT(birthdate) AS count_birthdays
               ^
HINT:  No function matches the given name and argument types. You might need to add explicit type casts.
```

What is the second issue?
The issue here is a typo in the function name. The SQL function to count the number of non-null values is COUNT, but in your code, it is mistakenly written as COUNNT. SQL is case-insensitive for keywords and function names, but the spelling must be correct.

Finally, the corrected version of the code is:
```SQL
SELECT COUNT(birthdate) AS count_birthdays
FROM people;
```

#### 2.1.3 SQL Style

- SQL is a generous language when it comes to formatting.
- New lines, capitalization, and indentation are not required in SQL as they sometimes are in other programming languages.
- While keyword capitalization and new lines are standard practice, many of the finer details of SQL style are not.
- For instance, some SQL users prefer to create a new line and indent each selected field when a query selects multiple fields, as the query on this slide does. For example:
```SQL
SELECT
    title,
    release_year,
    country
FROM films
LIMIT 3;
```

- Because of the different formatting styles, it's helpful to follow a SQL style guide, such as Holywell's, which outlines a standard of best practices for indentation, capitalization, and naming conventions for tables, fields, and aliases. (SQL Style Guide)[https://www.sqlstyle.guide/]
- Like capitalization and new lines, **semicolon** is unnecessary in PostgreSQL; we could leave it out of the query and still expect the same results with no errors.
- However, including a semicolon at the end of the query is considered best practice for several reasons:
  * First, some SQL flavors require it, so it's a good habit to have.
  * Including a semicolon in a PostgreSQL query means that the query is more easily translated to another flavor if necessary.
  * Additionally, like a period at the end of a sentence, a semicolon at the end of a query indicates its end, which is helpful in a file containing several queries.

- When creating a table, a SQL mistake is including spaces in a field name. For example:
```SQL
SELECT title, release year, country
FROM films
LIMIT 3;
```

- To query that table, we'll need to enclose the field name in double-quotes to indicate that, despite being two words, the name refers to just one field.
```SQL
SELECT title, "release year", country
FROM films
LIMIT 3;
```

### 2.2 Filtering Records

#### 2.2.1 Filtering Numbers

- To filter, we need to use a new clause called `WHERE`, which allows us to focus on only the data relevant to our business questions.
- In this lesson, we focus on filtering numbers. To do this, we use comparison operators such as greater than. For example:
```SQL
SELECT title
FROM films
WHERE release_year > 1960;
```

Or
```SQL
SELECT title
FROM films
WHERE release_year < 1960;
```

Or
```SQL
SELECT title
FROM films
WHERE release_year <= 1960;
```

Or
```SQL
SELECT title
FROM films
WHERE release_year = 1960;
```

Or
```SQL
SELECT title
FROM films
WHERE release_year <> 1960;
```
With this last example, we can filter so that we leave out just the observations that match the value we set. Basically, it helps us pick out everything except the ones that have that particular value.

- WHERE and the comparison operator, equals, can also be used with strings. In these cases, we will have to use single quotation marks around the strings we want to filter.
- Similar to `LIMIT`, this clause comes after the `FROM` statement when writing a query.
- If we use both `WHERE` and `LIMIT`, the written order will be `SELECT`, `FROM`, `WHERE`, `LIMIT`; however, the order of execution will now be `FROM`, `WHERE`, `SELECT`, `LIMIT`.
- Some other examples:
  * If we want to count the records with at least 100,000 votes, we use the following code: 
```SQL
SELECT COUNT(film_id) AS films_over_100K_votes
FROM reviews
WHERE num_votes >= 100000;
```
  * If we want to select `film_id` and `imdb_score` with an `imdb_score` over 7.0, we use the following code:
```SQL
SELECT film_id, imdb_score
FROM reviews
WHERE imdb_score > 7.0; 
```
  * If we want to select `film_id` and `facebook_likes` for ten records with less than 1000 likes, we use the following code:
```SQL
SELECT film_id, facebook_likes
FROM reviews
WHERE facebook_likes < 1000
LIMIT 10;
```

- Examples using `WHERE` with text:
  * If we want to select and count the `language` field using the alias `count_spanish` and then, apply a filter to select only `Spanish` from the `language` field. To do this, we use the following code:
```SQL
SELECT COUNT(language) AS count_spanish
FROM films
WHERE language = 'Spanish';
```
Note: It’s important to remember that when mentioning strings, you should use single quotes 'word' instead of double quotes "word". This is because SQL interprets double quotes "word" as a variable name, not the variable’s value.

#### 2.2.2 Multiple Criteria

- Three additional keywords that will allow us to enhance our filters when using `WHERE` by adding multiple criteria. These are:
  * `OR` -> is used when we want to filter **multiple criteria** and only need to satisfy at least one condition. For example:
```SQL
SELECT title
FROM films
WHERE release_year = 1994
    OR release_year = 2000;
```

  * `AND` -> is used when we want to satisfy all criteria in our filter.
```SQL
SELECT title
FROM films
WHERE release_year > 1994
    AND release_year < 2000;
```

  * `OR` + `AND` -> we can combine `AND` and `OR` if a query has multiple filtering conditions. **Important:** we will need to enclose the individual clauses in parentheses to ensure the correct execution order; otherwise, we may not get the expected results. For example:
```SQL
SELECT title
FROM films
WHERE (release_year = 1994 OR release_year = 1995)
    AND (certification = 'PG' OR certification = 'R');
```

  * `BETWEEN` -> provides a valuable shorthand for filtering values within a specified range. It's important to remember that BETWEEN is inclusive, meaning the results contain the beginning and end values. For example:
```SQL
SELECT title
FROM films
WHERE release_year
    BETWEEN 1994 AND 2000;
```

Notice that the previous code (query) is equivalent to the next one: 
```SQL
SELECT title
FROM films
WHERE release_year >= 1994
    AND release_year <= 2000; 
```

  * Like the `WHERE` clause, the `BETWEEN` clause can be used with multiple `AND` and `OR` operators, so we can build up our queries and make them even more powerful. For example:
```SQL
SELECT title
FROM films
WHERE release_year
BETWEEN 1994 AND 2000 AND country='UK';
```

  * In SQL, the `WHERE` clause is used to filter records, but it must be part of a `SELECT` statement to specify which columns to retrieve from the database.

#### 2.2.3 Filtering Text

- We can use `WHERE` clause to filter text data.
- We'll often want to search for a pattern rather than a specific text string in the real world.
- We'll be introducing three more SQL keywords into our vocabulary to help us achieve this:
  * `LIKE` -> we can use this operator with a `WHERE` clause to search for a pattern in a field.
    * We use a wildcard as a placeholder for some other values to accomplish this.
    * There are two wildcards with `LIKE`, the percent `%`, and the underscore `_`.
    * The percent `%` wildcard will match zero, one, or many characters in the text.
    * For example, the following query on the left matches people like Adel, Adelaide, and Aden:
    ```SQL
    SELECT name
    FROM people
    WNHERE name LIKE 'Ade%';
    ```
    * The underscore `_` wildcard will match a single character.
    * For example, the following query matches only three-letter names like Eve. We'd also see names like Eva if it were in our dataset. Eva Mendes, however, would not be visible unless the search criteria looked like this.
    ```SQL
    SELECT name
    FROM people
    WNHERE name LIKE 'Ev_';
    ```

  * `NOT LIKE` -> We can use this operator to find records that don't match the specified pattern. It's important to note that this operation is case-sensitive, so we must be mindful of what we are querying. For example, if we want to find records for people who do not have A-dot as part of their first name:
    ```SQL
    SELECT name
    FROM people
    WHERE name NOT LIKE 'A.%';
    ```
  * We've reviewed one example of where to position each wildcard, but we can actually put them anywhere and combine them.
  * We can find values that start, end, or contain characters in any position, as well as find records of a certain length. F
  * For example, the following code will find all people whose name ends in r:
  ```SQL
  SELECT name
  FROM people
  WHERE name LIKE '%r';
  ```
  
  * Another example, will find records where the third character is t:
  ```SQL
  SELECT name
  FROM people
  WHERE name LIKE '__t%';
  ```

  * `IN` -> allows us to specify multiple values in a `WHERE` clause, making it easier and quicker to set numerous `OR` conditions. For example:
  ```SQL
  SELECT title
  FROM films
  WHERE release_year IN(1920, 1930, 1940);
  ```

  ```SQL
  SELECT title
  FROM films
  WHERE country IN ('Germany', 'France');
  ```

- So far, our SQL vocabulary includes `COUNT()`, `DISTINCT`, `LIMIT`, `WHERE`, `OR`, `AND`, `BETWEEN`, `LIKE`, `NOT LIKE`, and `IN`.

- We have to create a code that:
  * Count the unique `titles` from the films database and use the alias provided.
  * Filter to include only movies with a `release_year` from 1990 to 1999, inclusive.
  * Filter to include only English-language films.
  * Filter to select only films with 'G', 'PG', 'PG-13' certifications.
```SQL
SELECT COUNT(DISTINCT title) AS nineties_english_films_for_teens
FROM films
WHERE release_year BETWEEN 1990 AND 1999
  AND language = 'English'
  AND certification IN ('G', 'PG', 'PG-13');
```

#### 2.2.4 Null Values

- In SQL, `NULL` represents a missing or unknown value.
- One quick way to see how much of our data is missing is by using `IS NULL` with the `WHERE` clause. For example:
```SQL
SELECT name
FROM people
WHERE birthdata IS NULL;
```

- If we want to count the missing birthdates in the people table, we use the code:
```SQL
SELECT COUNT(*) AS no_birthdates
FROM people
WHERE birthdate IS NULL;
```

- On the other hand, if we want to count results that are not `NULL`, we use the code:
```SQL
SELECT COUNT(*) AS no_birthdates
FROM people
WHERE birthdate IS NOT NULL;
```

### 2.3 Aggregate Functions

#### 2.3.1 Summarizing Data

- An **aggregate function** performs a calculation on several values and returns a single value. Normally, **aggregate functions** come after `SELECT`.
- Four new aggregate functions:
  * `AVG()` -> gives us the average value
  * `SUM()` -> gives us the total sum value
  * `MIN()` -> gives us the lowest value
  * `MAX()` -> gives us the greatest value
 
- Note that we operate ion the field (or column) with all of these aggregate functions, not the records (or rows).
- *Average* `AVG()` and *sum* `SUM()` are the two aggregate functions we can only use on numerical fields, since they requiere arithmetic. For example:
```SQL
SELECT AVG(budget)
FROM films;
```

Or
```SQL
SELECT SUM(BUDGET)
FROM films;
```

- We could also pass a negative number as the second parameter and still get a result. Here, the function is rounding to the left of the decimal point instead of the right.

#### 2.3.2 Summarizing Subsets

- We can combine aggregate functions with the WHERE clause to gain further insights from our data. That's because the WHERE clause executes before the SELECT statement. For example:
```SQL
SELECT AVG(budget) AS avg_budget
FROM films
WHERE release_year >= 2010;
```

- In SQL, we can use `ROUND()` to round our number to a specified decimal.
- There are two parameters for `ROUND(number_to_round, decimal_places)`: the number we want to round and the decimal place we want to round to.
- The second parameter in our ROUND() function is optional, so we can leave it out if we want to round to a whole number. 
- For example:
```SQL
SELECT ROUND(AVG(budget),2) AS avg_budget
FROM films
WHERE release_year >= 2010;
```

#### 2.3.3 Aliasing and Arithmetic

- We can perform basic arithmetic with symbols like plus `+`, minus `-`, multiply `*`, and divide `/`.
- Using parentheses with arithmetic indicates to the processor when the calculation needs to execute.
- For example:
```SQL
SELECT (4 + 3)
```

- When dividing, we can add decimal places to our numbers if we want more precision.
```SQL
SELECT (4.0 / 3.0)
```

- What's the difference between using aggregate functions and arithmetic? The key difference is that aggregate functions, like SUM, perform their operations on the fields vertically while arithmetic adds up the records horizontally.
- We will always need to use an alias when summarizing data with aggregate functions and arithmetic.
- A reminder of the order of execution we know so far: SQL will process the FROM statement first, followed by the WHERE clause, then the SELECT statement, and finally, LIMIT.
- When adding an alias for a field name in the SELECT clause, we might assume we could use it later in our query with the WHERE clause. Unfortunately, that is not possible; as we can see by the order of execution, the query would not have created the alias yet, and our code would generate an error.

### 2.4 Sorting and Grouping

#### 2.4.1 Sorting Results

- In SQL, the `ORDER BY` keyword is used to sort results of one or more fields. When used on its own, it is written after the `FROM` statement.
- `ORDER BY` will sort in ascending order by default. This can mean from smallest to biggest or from A to Z. For example:
```SQL
SELECT title, budget
FROM films
ORDER BY budget;
```

- We could also add the `ASC` keyword to our query to clarify that we are sorting in ascending order.
- We can use the `DESC` keyword to sort the results in descending order.
- Notice that we don't have to select the field we are sorting on.
- `ORDER BY` can also be used to sort on multiple fields.
  * It will sort by the first field specified, then sort by the next, etc.
  * To specify multiple fields, we separate the field names with a comma.
  * The second field we sort by can be thought of as a tie-breaker when the first field is not decisive in telling the order.

```SQL
SELECT title, wins
FROM best_movies
ORDER BY wins DESC;
```

```SQL
SELECT title, wins, imdb_score
FROM best_movies
ORDER BY wins DESC, imdb_score DESC;
```

- `ORDER BY` falls towards the end of the order of execution we already know, coming in just before limit. The `FROM` statement will execute first, then `WHERE`, followed by `SELECT`, `ORDER BY`, and finally, `LIMIT`.

#### 2.4.2 Grouping Data

- SQL allows us to group with the GROUP BY clause.
- GROUP BY is commonly used with aggregate functions to provide summary statistics.
- Example:
```SQL
SELECT certification, COUNT(title) AS title_count
FROM films
GROUP BY certification
```

- SQL will return an error if we try to SELECT a field that is not in our GROUP BY clause. We'll need to correct this by adding an aggregate function around title. For example:
- Wrong code:
```SQL
SELECT certification, title
FROM films
GROUP BY certification
```
- Corrected code:
```SQL
SELECT certification, COUNT(title) AS count_title
FROM films
GROUP BY certification;
```

- We can use GROUP BY on multiple fields similar to ORDER BY. The order in which we write the fields affects how the data is grouped.
```SQL
SELECT certification, language, COUNT(title) AS title_count
FROM films
GROUP BY certification, language;
```

- We can combine GROUP BY with ORDER BY to group our results, make a calculation, and then order our results. For example:
```SQL
SELECT certification, COUNT(title) AS title_count
FROM films
GROUP BY certification, language
ORDER BY title_count DESC;
```

- `ORDER BY` is always written after `GROUP BY`, and notice that we can refer back to the alias within the query. That is because of the order of execution.

- Other example: Select the `release_year`, `country`, and the maximum `budget` aliased as `max_budget` for each year and each country; sort your results by `release_year` and `country`.
```SQL
SELECT release_year, country, MAX(budget) AS max_budget
FROM films
GROUP BY release_year, country
ORDER BY release_year, country;
```

#### 2.4.3 Filtering Grouped Data

- In SQL, we can't filter aggregate functions with `WHERE` clauses.
- For example, this query attempting to filter the title count is invalid.
```SQL
SELECT release_year, COUNT(title) AS title_count
FROM films
GROUP BY release_year
WHERE COUNT(title) > 10;
```
```
syntax error at or near "WHERE"
LINE 4: WHERE COUNT(title) > 10;
        ^
```

- That means that if we want to filter based on the result of an aggregate function, we need another way.
- Groups have their own special filtering word: `HAVING`.
- For example, this query shows only those years in which more than ten films were released:
```SQL
SELECT release_year, COUNT(title) AS title_count
FROM films
GROUP BY release_year
HAVING COUNT(title) > 10;
```

- The reason why groups have their own keyword for filtering comes down to the order of execution.
- Example: Select country from the films table, and get the distinct count of certification aliased as certification_count. Group the results by country. Filter the unique count of certifications to only results greater than 10.
```SQL
-- Select the country and distinct count of certification as certification_count
SELECT country, COUNT(DISTINCT certification) AS certification_count
FROM films
-- Group by country
GROUP BY country
-- Filter results to countries with more than 10 different certifications
HAVING COUNT(DISTINCT certification) > 10
```

## PROJECT 1: Analyzing Students' Mental Health

Explore and analyze the `students` data to see how the length of stay (`stay`) impacts the average mental health diagnostic scores of the international students present in the study.
- Return a table with nine rows and five columns.
- The five columns should be aliased as: `stay`, `count_int`, `average_phq`, `average_scs`, and `average_as`, in that order.
- The average columns should contain the average of the `todep` (PHQ-9 test), `tosc` (SCS test), and `toas` (ASISS test) columns for each length of stay, rounded to two decimal places.
- The `count_int` column should be the number of international students for each length of stay.
- Sort the results by the length of stay in descending order.

Note: Creating new cells in the workbook will rename the DataFrame. Make sure that your final solution uses the name `df`.

Does going to university in a different country affect your mental health? A Japanese international university surveyed its students in 2018 and published a study the following year that was approved by several ethical and regulatory boards.

The study found that international students have a higher risk of mental health difficulties than the general population, and that social connectedness (belonging to a social group) and acculturative stress (stress associated with joining a new culture) are predictive of depression.

Explore the `students` data using PostgreSQL to find out if you would come to a similar conclusion for international students and see if the length of stay is a contributing factor.

Here is a data description of the columns you may find helpful.

| Field Name    | Description                                      |
| ------------- | ------------------------------------------------ |
| `inter_dom`     | Types of students (international or domestic)   |
| `japanese_cate` | Japanese language proficiency                    |
| `english_cate`  | English language proficiency                     |
| `academic`      | Current academic level (undergraduate or graduate) |
| `age`           | Current age of student                           |
| `stay`          | Current length of stay in years                  |
| `todep`         | Total score of depression (PHQ-9 test)           |
| `tosc`          | Total score of social connectedness (SCS test)   |
| `toas`          | Total score of acculturative stress (ASISS test) |

```sql
SELECT * 
FROM students;
```

```sql
SELECT stay, 
       COUNT(*) AS count_int,
       ROUND(AVG(todep), 2) AS average_phq, 
       ROUND(AVG(tosc), 2) AS average_scs, 
       ROUND(AVG(toas), 2) AS average_as
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay DESC;
```

## 3. Joining Data in SQL

### 3.1 Introducing Inner Joins

#### 3.1.1 The ins and outs of `INNER JOIN`

- Joining data is an essential skill which enables us to draw information from separate tables together into a single, meaningful set of results.
- `INNER JOIN`, which along with `LEFT JOIN` makes up one of the two most common joins.
- A **key** is a single column or group of columns that uniquely identifies records in a table.
- With SQL joins, you can join on a key field, or any other field.
- The **INNER JOIN** shown looks for records in both tables with the same values in the key field, id.
- The syntax for performing an `INNER JOIN` on our tables is shown step-by-step:
  * It is common to begin constructing the query with the join first, and selecting fields later.
  * After `FROM`, we list the left table, followed by the `INNER JOIN` keyword and the right table.
  * We then specify the field to match the tables on, using the `ON` keyword.
  * Lastly, we add `SELECT` at the start of the query and choose the fields we want returned.
  * When selecting columns that exist in both tables, the table-dot-column_name format must be used to avoid a SQL error.

```sql
SELECT prime_ministers.country, prime_ministers.continent, prime_minister, president
FROM presidents
INNER JOIN prime_ministers
ON  presidents.country = prime_ministers.country;
```

- When joining on two identical column names, we can employ the `USING` command followed by the shared column name in parentheses.

```sql
SELECT p2.country, p2.continent, prime_minister, president
FROM presidents AS p1
INNER JOIN prime_ministers AS p2
USING(country);
```

Examples:
- Perform an inner join with the cities table on the left and the countries table on the right; you do not need to alias tables here.
- Join ON the country_code and code columns, making sure you identify them with the correct table.

```sql
SELECT * 
FROM cities
-- Inner join to countries
INNER JOIN countries
-- Match on country codes
ON cities.country_code = countries.code;
```

- Complete the SELECT statement to keep three columns: the name of the city, the name of the country, and the region the country is located in (in this order).
- Alias the name of the city AS city and the name of the country AS country.

```sql
-- Select name fields (with alias) and region 
SELECT cities.name AS city, countries.name AS country, countries.region AS region
FROM cities
INNER JOIN countries
ON cities.country_code = countries.code;
```

- Use the country `code` field to complete the `INNER JOIN` with `USING`; do not change any alias names.

```sql
SELECT c.name AS country, l.name AS language, official
FROM countries AS c
INNER JOIN languages AS l
-- Match using the code column
USING(code)
```

#### 3.1.2 Defining Relationships

- The first type of relationship we'll talk about is a **one-to-many** relationship.
- This is the most common type of relationship, one where a single entity can be associated with several entities.
- A second type of relationship is a **one-to-one** relationship.
- **One-to-one** relationships imply unique pairings between entities and are therefore less common.
- The last type of relationship we'll discuss is a **many-to-many** relationship. 

Examples:

- Select the country `name`, aliased as `country`, from the `countries` table.

```sql
-- Select country (aliased) from countries
SELECT name AS country
FROM countries
```

- Now add an alias `c` for the `countries` table and perform an inner join with the `languages` table, `l`, on the right;
- Join on `code` in line 8 with the `USING` keyword; include the language name, aliased as `language`.

```sql
-- Select country and language names (aliased)
SELECT c.name AS country, l.name AS language
-- From countries (aliased)
FROM countries AS c
-- Join to languages (aliased)
INNER JOIN languages AS l
-- Use code as the joining field with the USING keyword
USING(code);
```

- Add a `WHERE` clause to find how many countries speak the language `'Bhojpuri'`.

```sql
-- Select country and language name (aliased)
SELECT c.name AS country, l.name AS language
-- From countries (aliased)
FROM countries AS c
-- Join to languages (aliased)
INNER JOIN languages AS l
-- Use code as the joining field with the USING keyword
USING(code)
-- Filter for the Bhojpuri language
WHERE l.name='Bhojpuri';
```

#### 3.1.3 Multiple Joins

- A powerful feature of SQL is that multiple joins can be combined and run in a single query.
- We begin with the same `INNER JOIN` as before, and then chain another `INNER JOIN` to the result of our first `INNER JOIN`.
- Notice that we use `left_table.id` in the last line of this example.
- If we want to perform the second join using the id field of `right_table` rather than `left_table`, we can replace `left_table.id` with `right_table.id` in the final line.

```sql
SELECT *
FROM left_table
INNER JOIN right_table
ON left_table.id = right_table.id
INNER JOIN another_table
ON left_table.id = another_table.id
```

- The `ON` clause in a `JOIN` statement requires a condition that evaluates to a boolean value.
- One more thing to know about joins is that it isn't always the case that each value in the field being joined on corresponds to exactly one record in the joining field of the right table.
- We can limit the records returned by supplying an additional field to join on by adding the AND keyword to our ON clause.

```sql
-- SQL query for chaining inner joins
SELECT p1.country, p1.continent, president, prime_minister, pm_start
FROM prime_ministers as p1
INNER JOIN presidents as p2
USING(country)
INNER JOIN prime_minister_terms as p3
USING(prime_minister);
```

```sql
SELECT *
FROM left_table
INNER JOIN right_table
ON left_table.id = right_table.id
  AND left_table.date = right_table.date;
```

**Examples:**

Suppose you are interested in the relationship between fertility and unemployment rates. Your task in this exercise is to join tables to return the country name, year, fertility rate, and unemployment rate in a single result from the `countries`, `populations` and `economies` tables.
  
1. Do an inner join of `countries AS c` (left) with `populations AS p` (right), on `code`.
   Select `name` and `fertility_rate`.

```sql
-- Select relevant fields
SELECT c.name, p.fertility_rate
FROM countries AS c
-- Inner join countries and populations, aliased, on code
INNER JOIN populations AS p
ON c.code = p.country_code;
```

2. Chain an inner join with the `economies` table `AS e`, on `code`.
   Select `year` and `unemployment_rate` from the `economies` table.

```sql
-- Select fields
SELECT name, e.year, fertility_rate, e.unemployment_rate
FROM countries AS c
INNER JOIN populations AS p
ON c.code = p.country_code
-- Join to economies (as e)
INNER JOIN economies AS e
-- Match on country code
ON c.code = e.code;
```

3. The last join was performed on `c.code = e.code`, without also joining on `year`.
   Your task in this exercise is to fix your query by explicitly stating that both the country `code` and `year` should match!

```sql
SELECT name, e.year, fertility_rate, unemployment_rate
FROM countries AS c
INNER JOIN populations AS p
ON c.code = p.country_code
INNER JOIN economies AS e
ON c.code = e.code
-- Add an additional joining condition such that you are also joining on year
	AND p.year = e.year;
```

### 3.2 Outer Joins, Cross Joins and Self Joins

#### 3.2.1 LEFT and RIGHT JOINs



#### 3.2.2 FULL JOINs



#### 3.2.3 Crossing into CROSS JOIN



#### 3.2.4 Self Joins



```sql

```

```sql

```

```sql

```

```sql

```

---

### 3.3 Set Theory for SQL Joins

#### 3.3.1 Set Theory for SQL Joins


#### 3.3.2 At the INTERSECT



#### 3.3.3 EXCEPT




### 3.4 Subqueries

---
## 4. Data Manipulation in SQL

### 4.1 We'll take the CASE


### 4.2 Short and Simple Queries


### 4.3 Correlated Queries, Nested Queries, and Common Table Expressions


### 4.4 Window Functions

---
## 5. PostgreSQL Summary Stats and Window Functions

### 5.1 Introduction to window functions


### 5.2 Fatching, Ranking, and Paging


### 5.3 Aggregate window functions and frames


### 5.4 Beyond window functions


---
## 6. Functions for Manipulting Data in PostgreSQL

### 6.1 Overview of Common Data Types


### 6.2 Working with DATE/TIME Functions and Operations


### 6.3 Parsing and Manipulating Text


### 6.4 Full-text Search and PostgresSQL Extensions


---
## 7. Introduction ton Statistics 

### 7.1 Summary Statistics


### 7.2 Probability and Distributions


### 7.3 More Distributions and the Central Limit Theorem


### 7.4 Correlation and Hypothesis Testing


---
## 8. Exploratory Data Analysis (EDA) in SQL

### 8.1 What's in the Database?


### 8.2 Summarizing and Aggregating Numeric Data


### 8.3 Exploring Categorical DAta and Unstructured Text


### 8.4 Working with Dates and Timestamps


---
## 9. Data-Driven Decision Making in SQL

### 9.1 Introduction to Business Intelligence for a Online Movie Rental Database

### 9.2 Decision Making with Simple SQL Queries

### 9.3 Data Driven Decision Making with Advanced SQL Queries

### 9.4 Data Driven Decision Making with OLAP SQL Queries

---
## 10. Understanding Data Visualization

### 10.1 Visualizing Distributions

### 10.2 Visualizing Two Variables

### 10.3 The Color and The Shape

### 10.4 99 Problems but a Plot ain't one of them

---
## 11. Data Communication Concepts 

### 11.1 Storytelling with Data

### 11.2 Preparing to Communicate the Data

### 11.3 Structuring Written Reports

### 11.4 Building Compelling Oral Presentations

---
