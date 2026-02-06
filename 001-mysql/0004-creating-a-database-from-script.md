# Creating a Database from an SQL Script in MySQL Workbench

Databases are often created using SQL scripts rather than being built manually table by table. An SQL script allows an entire database structure and its data to be defined in advance and created in a single, repeatable process. This approach is widely used in professional environments for consistency, automation, and reliability.

This guide explains how to create a database in MySQL Workbench by loading and executing an existing SQL script.

---

## What Is an SQL Script?

An SQL script is a file that contains a sequence of SQL statements written in advance. These statements may include commands to:

- Create a database
- Define tables and their columns
- Set relationships between tables
- Insert large amounts of data

When executed, MySQL processes the script from top to bottom, performing each operation in order.

---

## Why Use a Script to Create a Database?

Using an SQL script provides several advantages:

- The entire database can be recreated exactly the same way every time
- Large and complex schemas can be set up quickly
- Data and structure remain consistent across different systems
- Manual errors are avoided

Scripts are especially useful for training databases, sample projects, and shared development environments.

---

## Opening an SQL Script in MySQL Workbench

To load a database using an existing script, MySQL Workbench provides a built-in option.

1. Open MySQL Workbench and connect to the MySQL server.
2. From the top menu, select:
   - **File**
   - **Open SQL Script**
3. Browse to the location of the SQL script file and open it.

Once opened, the script will appear in the SQL editor window.

---

## What Happens When the Script Is Loaded

Loading the script prepares MySQL Workbench to execute the SQL statements, but it does not yet modify the database. At this stage:

- The SQL code becomes visible and editable
- MySQL Workbench is ready to run the script
- No tables or data are created until execution

---

## Executing the Script

When the script is executed, MySQL processes each statement in sequence.

Typically, the script performs two major actions:

1. **Creating the Database Structure**
   - A database is created if it does not already exist
   - Tables are defined with columns, data types, and constraints
   - Relationships between tables are established

2. **Loading Data into the Tables**
   - Rows of data are inserted into each table
   - The database is populated and ready for querying

This process can take some time depending on the size of the database and the amount of data being inserted.

---

## Verifying the Database Creation

After execution:
- The new database appears in the Schemas panel
- Tables are listed under the database name
- Data is available for querying immediately

The database is now fully constructed and ready for use.

---

## Summary

Creating a database from an SQL script is a standard and efficient approach used in real-world development. By loading and executing a script in MySQL Workbench, both the database structure and its data can be created in a single, controlled process. This method ensures consistency, reduces setup time, and provides a reliable foundation for working with SQL.
