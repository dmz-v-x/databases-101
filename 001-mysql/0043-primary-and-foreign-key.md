# Primary Keys and Foreign Keys in MySQL — Complete Step-by-Step Guide

Primary keys and foreign keys are the **backbone of relational databases**. They define identity, enforce relationships, and protect data integrity. If these two concepts are understood properly, everything else in SQL—joins, constraints, normalization—starts to feel natural.

This guide explains **what they are, why they exist, how to use them, and common mistakes**, in a clear and ordered way.

---

## Why Keys Exist in Relational Databases

Relational databases store data in **multiple tables** instead of one big table. To make this work safely:

- Every row must be uniquely identifiable
- Relationships between tables must be enforced
- Invalid or orphaned data must be prevented

This is exactly what **primary keys** and **foreign keys** do.

---

## Primary Key

### What Is a Primary Key?

A **primary key** is a column (or set of columns) that **uniquely identifies each row in a table**.

Rules of a primary key:
- Must be **unique**
- Cannot be **NULL**
- Only **one primary key per table**
- Identifies a row forever

Think of it as the **identity card** of a row.

---

## Why Primary Keys Are Required

Primary keys allow MySQL to:
- Identify rows unambiguously
- Enforce relationships
- Index data efficiently
- Prevent duplicate records

Every table should have a primary key.

---

## Creating a Primary Key

### Single-Column Primary Key

    CREATE TABLE customers (
        customer_id INT PRIMARY KEY,
        first_name VARCHAR(50),
        last_name VARCHAR(50)
    );

Here:
- `customer_id` uniquely identifies each customer

---

### Primary Key with AUTO_INCREMENT

This is the most common real-world pattern.

    CREATE TABLE customers (
        customer_id INT AUTO_INCREMENT PRIMARY KEY,
        email VARCHAR(100)
    );

MySQL:
- Automatically generates unique IDs
- Prevents manual ID mistakes

---

## Composite Primary Key

A **composite primary key** uses multiple columns together.

    CREATE TABLE order_items (
        order_id INT,
        product_id INT,
        quantity INT,
        PRIMARY KEY (order_id, product_id)
    );

Here:
- The combination of `order_id` and `product_id` must be unique
- Neither column alone is sufficient

Used commonly in junction tables.

---

## Foreign Key

### What Is a Foreign Key?

A **foreign key** is a column (or set of columns) that **references the primary key of another table**.

It creates a relationship between tables.

Rules:
- Foreign key values must exist in the referenced table
- Or be NULL (if allowed)
- Enforces referential integrity

Think of it as a **link or pointer** to another table.

---

## Why Foreign Keys Are Important

Foreign keys prevent:
- Orphan records
- Invalid references
- Broken relationships

They ensure:
- Data consistency
- Logical correctness
- Safe deletes and updates

---

## Creating a Foreign Key

### Basic Foreign Key Example

    CREATE TABLE orders (
        order_id INT AUTO_INCREMENT PRIMARY KEY,
        customer_id INT,
        FOREIGN KEY (customer_id)
            REFERENCES customers(customer_id)
    );

This enforces:
- Every order must belong to a valid customer
- You cannot insert an order with a non-existent customer_id

---

## Foreign Keys with NOT NULL

    customer_id INT NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)

This ensures:
- Every order must have a customer
- No orphan orders are possible

---

## ON DELETE and ON UPDATE Rules

Foreign keys can define behavior when the parent row changes.

---

### ON DELETE RESTRICT (Default)

    ON DELETE RESTRICT

Prevents deleting a parent row if child rows exist.

---

### ON DELETE CASCADE

    ON DELETE CASCADE

Automatically deletes child rows when the parent is deleted.

Example:
- Delete customer → all orders deleted

Use with caution.

---

### ON DELETE SET NULL

    ON DELETE SET NULL

When the parent row is deleted:
- Foreign key value becomes NULL

Only works if the column allows NULL.

---

### ON UPDATE CASCADE

    ON UPDATE CASCADE

If the primary key changes:
- Foreign keys update automatically

Rarely used, since primary keys usually do not change.

---

## Composite Foreign Keys

Foreign keys can reference composite primary keys.

    FOREIGN KEY (order_id, product_id)
    REFERENCES order_items(order_id, product_id)

All referenced columns must match in number and order.

---

## Adding Keys to Existing Tables

### Add Primary Key

    ALTER TABLE customers
    ADD PRIMARY KEY (customer_id);

---

### Add Foreign Key

    ALTER TABLE orders
    ADD CONSTRAINT fk_orders_customers
    FOREIGN KEY (customer_id)
    REFERENCES customers(customer_id);

---

## Removing Keys

### Drop Primary Key

    ALTER TABLE customers
    DROP PRIMARY KEY;

---

### Drop Foreign Key

    ALTER TABLE orders
    DROP FOREIGN KEY fk_orders_customers;

Foreign key names must be known to drop them.

---

## Common Mistakes and Gotchas

- Forgetting to define a primary key
- Using business data (email, phone) as primary keys
- Dropping parent tables before child tables
- Using CASCADE without understanding consequences
- Mismatched data types between PK and FK
- Assuming foreign keys are optional

---

## Best Practices

- Always define a primary key
- Use surrogate keys (`AUTO_INCREMENT`) for PKs
- Use foreign keys to enforce relationships
- Keep PKs stable (never update them)
- Use CASCADE rules intentionally
- Let the database enforce integrity, not just the app

---

## Primary Key vs Foreign Key (Quick Comparison)

| Feature | Primary Key | Foreign Key |
|------|------------|-------------|
| Purpose | Identify rows | Create relationships |
| Uniqueness | Required | Not required |
| NULL allowed | No | Yes (if allowed) |
| Count per table | One | Many |
| References | Itself | Another table |

---

## Mental Model

- **Primary key**: “Who are you?”
- **Foreign key**: “Who do you belong to?”

Primary keys define **identity**.  
Foreign keys define **relationships**.

Together, they turn a collection of tables into a **true relational database** that is consistent, reliable, and safe to scale.
