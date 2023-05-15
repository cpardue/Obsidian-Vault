## Data Definition Language (DDL)

-   `CREATE DATABASE`: Create a new database
-   `ALTER DATABASE`: Modify a database
-   `DROP DATABASE`: Delete a database
-   `CREATE TABLE`: Create a new table
-   `ALTER TABLE`: Modify a table
-   `DROP TABLE`: Delete a table
-   `TRUNCATE TABLE`: Delete all data from a table

## Data Manipulation Language (DML)

-   `SELECT`: Retrieve data from a table
-   `INSERT INTO`: Insert new data into a table
-   `UPDATE`: Modify existing data in a table
-   `DELETE FROM`: Delete data from a table

## Data Control Language (DCL)

-   `GRANT`: Give a user permission to perform a specific task
-   `REVOKE`: Remove a user's permission to perform a specific task

## Transaction Control Language (TCL)

-   `COMMIT`: Save changes to the database
-   `ROLLBACK`: Undo changes to the database

## Query Modifiers

-   `DISTINCT`: Select unique rows
-   `WHERE`: Filter the result set based on a condition
-   `GROUP BY`: Group the result set by one or more columns
-   `HAVING`: Filter the result set based on a condition applied to a group
-   `ORDER BY`: Sort the result set by one or more columns

## Joins

-   `INNER JOIN`: Return rows where there is a match in both tables
-   `LEFT JOIN`: Return all rows from the left table, and any matching rows from the right table

## Aggregate Functions

-   `AVG()`: Calculate the average value of a column
-   `COUNT()`: Count the number of rows in a table
-   `MAX()`: Find the maximum value of a column
-   `MIN()`: Find the minimum value of a column
-   `SUM()`: Calculate the sum of a column

## Subqueries

-   `SELECT * FROM (SELECT * FROM table1)`: Use the result of a SELECT statement as the input for another SELECT statement

## Views

-   `CREATE VIEW`: Create a new view
-   `ALTER VIEW`: Modify a view
-   `DROP VIEW`: Delete a view

## Stored Procedures

-   `CREATE PROCEDURE`: Create a new stored procedure
-   `ALTER PROCEDURE`: Modify a stored procedure
-   `DROP PROCEDURE`: Delete a stored procedure
-   `EXECUTE`: Execute a stored procedure

## Triggers

-   `CREATE TRIGGER`: Create a new trigger
-   `ALTER TRIGGER`: Modify a trigger
-   `DROP TRIGGER`: Delete a trigger