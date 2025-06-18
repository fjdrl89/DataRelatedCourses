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

#### 2.1.2 Query Execution


#### 2.1.3 SQL Style


### 2.2 Filtering Records




### 2.3 Aggregate Functions



### 2.4 Sorting and Grouping


## PROJECT 1: Analyzing Students' Mental Health

---
## 3. Joining Data in SQL

### 3.1 Introducing Inner Joins


### 3.2 Outer Joins, Cross Joins and Self Joins


### 3.3 Set Theory for SQL Joins


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
