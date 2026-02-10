# Indexes in MySQL — What They Are, How They Work, and How to Use Them

Indexes are one of the most important performance tools in MySQL. They do **not change your data**, but they drastically change **how fast MySQL can find data**. Understanding indexes is essential for backend and full-stack developers, even if you are not a database administrator.

This guide explains indexes from first principles, step by step, in a practical and ordered way.

---

## What Is an Index?

An **index** is a data structure that MySQL uses to **find rows faster**.

Without an index:
- MySQL scans every row in a table (full table scan)

With an index:
- MySQL uses a lookup structure to jump directly to matching rows

An index is similar to:
- An index in a book
- A phone directory
- A sorted lookup table

---

## Why Indexes Exist

Indexes solve one main problem: **speed**.

As tables grow:
- Queries become slower
- Full table scans become expensive
- Joins become costly

Indexes allow MySQL to:
- Locate rows quickly
- Join tables efficiently
- Sort and filter data faster

---

## What Indexes Do NOT Do

Indexes:
- Do NOT change query results
- Do NOT store new data (they reference existing data)
- Do NOT make every query faster
- Do NOT replace good schema design

Indexes are a performance optimization, not a logic feature.

---

## How Indexes Work Internally (Conceptual)

Internally, MySQL (InnoDB) uses **B-tree structures** for most indexes.

Conceptually:
- Index stores column values in sorted order
- Each value points to the actual row location
- MySQL navigates the tree instead of scanning rows

You do **not** need to understand B-trees deeply to use indexes correctly.

---

## Primary Key Index

### Primary Key Is an Index

Every `PRIMARY KEY` automatically creates an index.

    CREATE TABLE customers (
        customer_id INT AUTO_INCREMENT PRIMARY KEY,
        email VARCHAR(100)
    );

Here:
- `customer_id` is indexed automatically
- Lookups by `customer_id` are fast

---

## When You Need an Index

You should consider an index when:
- A column is frequently used in `WHERE`
- A column is used in `JOIN`
- A column is used in `ORDER BY`
- A column is used in `GROUP BY`
- The table is large

You usually do **not** need an index when:
- The table is small
- The column has very few unique values
- The column is rarely queried

---

## Creating an Index

### Basic Index Creation

    CREATE INDEX idx_email
    ON customers(email);

This creates:
- A non-unique index on `email`
- Faster lookups by email

---

## Creating a UNIQUE Index

    CREATE UNIQUE INDEX idx_email_unique
    ON customers(email);

This:
- Improves lookup speed
- Prevents duplicate values

Equivalent to:

    email VARCHAR(100) UNIQUE

---

## Creating a Composite (Multi-Column) Index

    CREATE INDEX idx_customer_date
    ON orders(customer_id, order_date);

This index helps queries like:

    WHERE customer_id = 5
    AND order_date > '2024-01-01';

Order matters:
- Leftmost column must be used first
- This is called the **leftmost prefix rule**

---

## Indexes and JOINs

Indexes are critical for joins.

Example:

    SELECT *
    FROM orders o
    JOIN customers c
      ON o.customer_id = c.customer_id;

Best practice:
- Primary key on `customers.customer_id`
- Index on `orders.customer_id`

Without indexes, joins become slow very quickly.

---

## Viewing Indexes on a Table

    SHOW INDEX FROM customers;

This shows:
- Index name
- Indexed columns
- Whether the index is unique
- Index type

---

## Dropping an Index

    DROP INDEX idx_email
    ON customers;

 importing removes the index but keeps the data.

---

## Indexes and INSERT / UPDATE / DELETE

Indexes speed up reads but slow down writes.

Why:
- Indexes must be updated when data changes

Trade-off:
- More indexes → faster SELECTs
- More indexes → slower INSERT/UPDATE/DELETE

Never add indexes blindly.

---

## Using EXPLAIN to See Index Usage

    EXPLAIN
    SELECT *
    FROM customers
    WHERE email = 'john@example.com';

This shows:
- Whether an index is used
- Which index is chosen
- How many rows MySQL expects to scan

If `type` is:
- `ALL` → full table scan (bad for large tables)
- `ref`, `range`, `const` → index is used (good)

---

## Common Index Mistakes

- Indexing every column
- Indexing columns with very low cardinality
- Ignoring composite index order
- Forgetting indexes on foreign keys
- Assuming indexes fix bad queries
- Creating indexes without measuring impact

---

## Indexes and Foreign Keys

Foreign keys should almost always be indexed.

    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)

MySQL automatically indexes foreign keys if needed.

This improves:
- JOIN performance
- DELETE / UPDATE checks

---

## Best Practices for Indexes

- Always index primary keys
- Index foreign keys
- Index columns used in WHERE and JOIN
- Use composite indexes thoughtfully
- Avoid over-indexing
- Measure with EXPLAIN
- Optimize based on real queries

---

## Index vs Full Table Scan (Mental Model)

Without index:
- MySQL checks every row

With index:
- MySQL jumps directly to matching rows

Indexes turn SQL from **linear scanning** into **logarithmic lookup**.

---

## Final Mental Model

- Tables store data
- Indexes help find data
- Primary keys are indexed automatically
- Foreign keys should be indexed
- Indexes speed up reads, slow down writes

Indexes are one of the highest-impact performance tools in MySQL. Used correctly, they make databases scale smoothly. Used incorrectly, they waste resources. Knowing when and why to use them is what separates beginners from professional developers.
