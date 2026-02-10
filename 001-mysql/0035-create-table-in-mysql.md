# Creating a Table in MySQL — Step-by-Step Guide

Creating a table is the moment where a database actually starts storing real data. A table defines **what data is stored**, **how it is structured**, and **what rules apply to that data**.

This guide explains what a table is, how `CREATE TABLE` works, and how to create tables correctly in MySQL.

---

## What Is a Table in MySQL?

A table is a structured collection of data made up of:
- Columns (fields)
- Rows (records)

Each table represents **one real-world entity** such as:
- customers
- orders
- products
- payments

A table always belongs to a **database (schema)** and cannot exist on its own.

---

## Prerequisite: Select a Database

Before creating a table, a database must be selected.

    USE myfirstcodesschema;

If no database is selected, MySQL will return:
`No database selected`

---

## Basic CREATE TABLE Syntax

    CREATE TABLE table_name (
        column_name data_type constraints,
        column_name data_type constraints
    );

Each column definition includes:
- Column name
- Data type
- Optional constraints

---

## Example: Creating a Simple Table

    CREATE TABLE customers (
        customer_id INT,
        first_name VARCHAR(50),
        last_name VARCHAR(50),
        email VARCHAR(100)
    );

This creates:
- A table named `customers`
- Four columns
- No constraints yet

---

## Adding a Primary Key

A primary key uniquely identifies each row in a table.

    CREATE TABLE customers (
        customer_id INT PRIMARY KEY,
        first_name VARCHAR(50),
        last_name VARCHAR(50),
        email VARCHAR(100)
    );

Rules of a primary key:
- Must be unique
- Cannot be NULL
- Only one per table

---

## Using AUTO_INCREMENT for IDs

In MySQL, `AUTO_INCREMENT` automatically generates unique values.

    CREATE TABLE customers (
        customer_id INT AUTO_INCREMENT PRIMARY KEY,
        first_name VARCHAR(50),
        last_name VARCHAR(50),
        email VARCHAR(100)
    );

This is the most common pattern for ID columns.

---

## Adding NOT NULL Constraints

`NOT NULL` ensures a column always has a value.

    CREATE TABLE customers (
        customer_id INT AUTO_INCREMENT PRIMARY KEY,
        first_name VARCHAR(50) NOT NULL,
        last_name VARCHAR(50) NOT NULL,
        email VARCHAR(100) NOT NULL
    );

This prevents incomplete records.

---

## Adding UNIQUE Constraints

`UNIQUE` prevents duplicate values in a column.

    CREATE TABLE customers (
        customer_id INT AUTO_INCREMENT PRIMARY KEY,
        email VARCHAR(100) UNIQUE,
        first_name VARCHAR(50),
        last_name VARCHAR(50)
    );

This ensures no two customers share the same email.

---

## Adding DEFAULT Values

`DEFAULT` assigns a value when none is provided.

    CREATE TABLE customers (
        customer_id INT AUTO_INCREMENT PRIMARY KEY,
        is_active TINYINT DEFAULT 1,
        created_at DATETIME DEFAULT CURRENT_TIMESTAMP
    );

Defaults help reduce errors and simplify inserts.

---

## Creating a Table with Different Data Types

    CREATE TABLE orders (
        order_id INT AUTO_INCREMENT PRIMARY KEY,
        customer_id INT NOT NULL,
        order_date DATE,
        total_amount DECIMAL(10,2),
        status ENUM('pending', 'completed', 'cancelled')
    );

This table uses:
- Integer IDs
- Dates
- Decimal money values
- Enum status values

---

## Creating Tables with Relationships (Foreign Key)

Foreign keys link tables together.

    CREATE TABLE orders (
        order_id INT AUTO_INCREMENT PRIMARY KEY,
        customer_id INT NOT NULL,
        order_date DATE,
        total_amount DECIMAL(10,2),
        FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
    );

This ensures:
- Orders always belong to a valid customer
- Referential integrity is enforced

---

## Viewing the Table Structure

To inspect a table:

    DESCRIBE customers;

or

    SHOW CREATE TABLE customers;

This shows column definitions, constraints, and indexes.

---

## Common CREATE TABLE Mistakes

- Forgetting to select a database
- Using incorrect data types
- Not defining a primary key
- Allowing NULL values unintentionally
- Using VARCHAR where INT or DATE is appropriate

---

## Best Practices for Creating Tables

- Always define a primary key
- Use meaningful column names
- Choose correct data types
- Use `NOT NULL` where appropriate
- Use `DECIMAL` for money
- Avoid overusing `ENUM` in evolving systems

---

## Mental Model

Think of a table as a blueprint:

- Columns define what data looks like
- Constraints define rules
- Rows are actual data instances

Once tables are designed correctly, querying, joining, and scaling the database becomes predictable and reliable.
