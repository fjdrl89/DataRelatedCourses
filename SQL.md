# Analista de Datos Asociado en SQL

## 1. Introducción a SQL 

### 1.1 Bases de Datos Relacionales

#### 1.1.1 Databases


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
- 


#### 1.2.3 SQL Flavors








## 2. SQL Intermedio Intermedio


## 3. Unir Datos en SQL

## 4. Manipulación de Datos en SQL

## 5. Estadísticas de Resumen y Funciones de Ventana de PostgreSQL

## 6. Funciones para Manipular Datos en PostgreSQL

## 7. Introducción a la Estadística

## 8. Análisis Exploratorio de Datos en SQL

## 9. Toma de Decisiones basada en Datos con SQL

## 10. Comprender la visualización de datos

## 11. conceptos de comunicación de datos
 
