# Constraints in MySQL — Complete Guide from Fundamentals to Practice

Constraints are **rules applied to table columns** that enforce data integrity. They ensure that the data stored in a database is **accurate, consistent, and reliable**. Without constraints, a database quickly becomes untrustworthy.

This guide explains **all major MySQL constraints**, what they do, when to use them, how to define them, and common mistakes.

---

## What Are Constraints?

A constraint is a rule that restricts what values can be inserted, updated, or deleted in a table.

Constraints help MySQL:
- Prevent invalid data
- Enforce relationships between tables
- Maintain consistency automatically
- Catch errors early

Constraints are enforced **by the database**, not the application.

---

## Why Constraints Matter

Without constraints:
- Duplicate records creep in
- Invalid references appear
- NULL values appear where they should not
- Data becomes unreliable

With constraints:
- The database protects itself
- Bugs are caught early
- Data remains trustworthy

---

## Types of Constraints in MySQL

MySQL supports the following major constraints:

- `PRIMARY KEY`
- `FOREIGN KEY`
- `UNIQUE`
- `NOT NULL`
- `DEFAULT`
- `CHECK` (limited support historically, enforced in modern MySQL)

---

## PRIMARY KEY Constraint

### What It Does

- Uniquely identifies each row
- Cannot be NULL
- Must be unique
- Only one per table

### Example

    CREATE TABLE customers (
        customer_id INT PRIMARY KEY,
        email VARCHAR(100)
    );

### Using AUTO_INCREMENT with PRIMARY KEY

    customer_id INT AUTO_INCREMENT PRIMARY KEY

This is the most common pattern for IDs.

---

## UNIQUE Constraint

### What It Does

- Prevents duplicate values
- Allows NULL (unless combined with NOT NULL)
- Multiple UNIQUE constraints allowed per table

### Example

    CREATE TABLE users (
        user_id INT PRIMARY KEY,
        email VARCHAR(100) UNIQUE
    );

This ensures no two users share the same email.

---

## NOT NULL Constraint

### What It Does

- Prevents NULL values in a column
- Forces data to always be present

### Example

    CREATE TABLE employees (
        employee_id INT PRIMARY KEY,
        first_name VARCHAR(50) NOT NULL
    );

Attempting to insert NULL will fail.

---

## DEFAULT Constraint

### What It Does

- Assigns a value when none is provided
- Prevents missing or inconsistent data

### Example

    CREATE TABLE orders (
        order_id INT PRIMARY KEY,
        status VARCHAR(20) DEFAULT 'pending'
    );

If `status` is omitted during insert, `'pending'` is used.

---

## FOREIGN KEY Constraint

### What It Does

- Enforces relationships between tables
- Prevents orphaned records
- Ensures referenced rows exist

### Example

    CREATE TABLE orders (
        order_id INT PRIMARY KEY,
        customer_id INT,
        FOREIGN KEY (customer_id)
            REFERENCES customers(customer_id)
    );

This ensures every order belongs to a valid customer.

---

## ON DELETE and ON UPDATE Rules

Foreign keys can define behavior when referenced data changes.

### ON DELETE CASCADE

    ON DELETE CASCADE

Deletes child rows automatically when parent is deleted.

### ON DELETE RESTRICT

Prevents deletion if related rows exist.

### ON DELETE SET NULL

Sets foreign key to NULL when parent is deleted.

---

## CHECK Constraint

### What It Does

- Enforces logical conditions on column values
- Ensures data falls within allowed rules

### Example

    CREATE TABLE payments (
        payment_id INT PRIMARY KEY,
        amount DECIMAL(10,2),
        CHECK (amount > 0)
    );

Negative payments are rejected.

> Note: CHECK is enforced only in modern MySQL versions (8.0+).

---

## Column-Level vs Table-Level Constraints

### Column-Level

Defined inline with the column.

    email VARCHAR(100) UNIQUE NOT NULL

### Table-Level

Defined after column definitions.

    UNIQUE (email)

Used for:
- Composite constraints
- Multi-column uniqueness

---

## Composite Constraints

Constraints can span multiple columns.

### Composite PRIMARY KEY

    PRIMARY KEY (order_id, product_id)

### Composite UNIQUE

    UNIQUE (email, phone_number)

These enforce rules across column combinations.

---

## Adding Constraints to Existing Tables

    ALTER TABLE users
    ADD UNIQUE (email);

    ALTER TABLE orders
    ADD CONSTRAINT fk_customer
    FOREIGN KEY (customer_id)
    REFERENCES customers(customer_id);

---

## Removing Constraints

    ALTER TABLE users
    DROP INDEX email;

    ALTER TABLE orders
    DROP FOREIGN KEY fk_customer;

Always verify before dropping constraints.

---

## Common Constraint Mistakes

- Forgetting PRIMARY KEY
- Using UNIQUE instead of PRIMARY KEY
- Allowing NULL unintentionally
- Overusing ENUM instead of CHECK
- Dropping constraints without understanding impact

---

## Best Practices for Constraints

- Always define a PRIMARY KEY
- Use NOT NULL intentionally
- Use UNIQUE for natural identifiers (email, username)
- Enforce relationships with FOREIGN KEY
- Let the database enforce rules, not just the app
- Keep constraints simple and meaningful

---

## Constraints vs Application Validation

| Database Constraints | Application Validation |
|---------------------|------------------------|
| Always enforced | Can be bypassed |
| Protect data integrity | Protect user experience |
| Prevent corruption | Improve usability |

Both should be used together.

---

## Mental Model

Think of constraints as **guards at the gate**:

- PRIMARY KEY checks identity
- UNIQUE prevents duplication
- NOT NULL ensures presence
- FOREIGN KEY enforces relationships
- CHECK enforces logic

Constraints make your database **self-defending**, reliable, and production-ready.
