# Inserting Records into a Table in MySQL

Inserting records is how data actually enters a MySQL table. The `INSERT` statement is used to add **new rows** into an existing table. Understanding how `INSERT` works is essential for backend development, data loading, and testing.

This guide explains all common ways to insert data, step by step, with clear examples and best practices.

---

## Prerequisite: Table Must Exist

Before inserting data:
- A database (schema) must exist
- A table must already be created
- You must be connected to the correct database

Example:

    USE myfirstcodesschema;

---

## Basic INSERT Syntax

The most common and recommended form of `INSERT` explicitly specifies column names.

    INSERT INTO table_name (column1, column2, column3)
    VALUES (value1, value2, value3);

This approach is:
- Clear
- Safe
- Resilient to schema changes

---

## Example: Inserting a Single Record

    INSERT INTO customers (first_name, last_name, email)
    VALUES ('John', 'Doe', 'john.doe@example.com');

This inserts:
- One new row
- Values mapped exactly to the specified columns

---

## Inserting Without Specifying Columns (Not Recommended)

    INSERT INTO customers
    VALUES (1, 'John', 'Doe', 'john.doe@example.com');

This relies on:
- Exact column order
- All columns being present

This is fragile and breaks easily if the table structure changes.

---

## Inserting Multiple Records at Once

MySQL allows inserting multiple rows in a single statement.

    INSERT INTO customers (first_name, last_name, email)
    VALUES
        ('Alice', 'Smith', 'alice@example.com'),
        ('Bob', 'Johnson', 'bob@example.com'),
        ('Carol', 'Brown', 'carol@example.com');

Benefits:
- Faster than multiple single inserts
- Cleaner scripts
- Common in data loading

---

## Inserting with AUTO_INCREMENT Columns

If a column is defined as `AUTO_INCREMENT`, it should be omitted from the insert.

    INSERT INTO customers (first_name, last_name, email)
    VALUES ('David', 'Wilson', 'david@example.com');

MySQL automatically generates the ID.

---

## Inserting with DEFAULT Values

If a column has a `DEFAULT` value, it can be omitted.

    INSERT INTO customers (first_name, last_name)
    VALUES ('Emma', 'Taylor');

The default value is automatically applied.

---

## Inserting NULL Values

To explicitly insert `NULL`:

    INSERT INTO customers (first_name, last_name, email)
    VALUES ('Frank', 'Miller', NULL);

This works only if the column allows `NULL`.

---

## Inserting Dates and DateTime Values

### DATE

    INSERT INTO orders (order_date)
    VALUES ('2024-01-15');

### DATETIME

    INSERT INTO orders (order_date)
    VALUES ('2024-01-15 10:30:00');

Dates must follow proper MySQL formats.

---

## Inserting Data from Another Table (INSERT INTO SELECT)

Data can be inserted using a query result.

    INSERT INTO archived_orders (order_id, order_date, total_amount)
    SELECT order_id, order_date, total_amount
    FROM orders
    WHERE order_date < '2020-01-01';

This is commonly used for:
- Archiving data
- Data migration
- ETL processes

---

## Handling Duplicate Records

### INSERT IGNORE

    INSERT IGNORE INTO customers (email)
    VALUES ('john.doe@example.com');

If a duplicate key error occurs, MySQL skips the row instead of failing.

---

### ON DUPLICATE KEY UPDATE

    INSERT INTO customers (email, first_name)
    VALUES ('john.doe@example.com', 'John')
    ON DUPLICATE KEY UPDATE first_name = 'John';

If the record exists:
- The row is updated instead of inserted

---

## Common INSERT Errors

- Column count does not match value count
- Violating NOT NULL constraints
- Duplicate primary or unique keys
- Invalid data types
- Date format mismatches

Reading error messages carefully usually reveals the issue.

---

## Best Practices for INSERT Statements

- Always specify column names
- Insert multiple rows when possible
- Avoid relying on column order
- Validate data before inserting
- Use transactions for critical inserts
- Never insert test data into production

---

## Typical Backend Workflow

1. Validate input data
2. Insert record
3. Capture generated ID (if needed)
4. Continue application logic

---

## Mental Model

Think of `INSERT` as placing a new record into a structured container:

- Columns define the slots
- Values must match those slots
- Constraints enforce rules

Once inserted, the record becomes part of the database and is immediately available for querying, joining, and analysis.
