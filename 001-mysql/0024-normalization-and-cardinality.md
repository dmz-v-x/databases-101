# Normalization and Cardinality in Databases

Normalization and cardinality are two foundational concepts in relational database design. They address different but closely related concerns: **how data is structured** and **how tables relate to each other**. Understanding both is essential for building databases that are efficient, consistent, and scalable.

---

## Normalization

### What Is Normalization?

Normalization is the process of organizing data in a database to:
- Reduce data redundancy
- Improve data integrity
- Eliminate update, insert, and delete anomalies

Normalization achieves this by breaking large tables into smaller, well-structured tables and defining clear relationships between them.

The goal is not to make queries complex, but to make the data model **logically sound and reliable**.

---

## Why Normalization Is Necessary

Without normalization, databases often suffer from:
- Duplicate data stored in multiple places
- Inconsistent updates (same data changed in one place but not another)
- Difficulty enforcing rules and constraints
- Larger storage requirements

Normalization ensures that each piece of data has **one clear home**.

---

## The Normal Forms (Overview)

Normalization is typically discussed in stages called **normal forms**. Each form builds on the previous one.

---

### First Normal Form (1NF)

Rules:
- Each column contains atomic (indivisible) values
- No repeating groups or multi-valued columns
- Each row can be uniquely identified

Example of violation:
- A single column storing multiple phone numbers

Corrected approach:
- One phone number per row

---

### Second Normal Form (2NF)

Rules:
- Must already be in 1NF
- All non-key columns must depend on the entire primary key

This matters mainly for tables with **composite primary keys**.

Example issue:
- A table where part of the key determines some columns, but not others

Solution:
- Split the table so each fact depends on the full key

---

### Third Normal Form (3NF)

Rules:
- Must already be in 2NF
- No transitive dependencies
- Non-key columns depend only on the primary key, not on other non-key columns

Example issue:
- Storing customer city and customer zip code together when zip code determines city

Solution:
- Move zip code–city relationship into its own table

---

## Practical View of Normalization

In practice, normalization means:
- One table per real-world entity
- Clear primary keys
- Foreign keys to express relationships
- Minimal duplication of data

Most production systems aim for **3NF**, sometimes relaxing it slightly for performance reasons.

---

## Cardinality

### What Is Cardinality?

Cardinality describes **how many records in one table relate to records in another table**.

It defines the nature of relationships between tables.

---

## Types of Cardinality Relationships

---

### One-to-One (1:1)

Each row in Table A relates to exactly one row in Table B.

Example:
- A user table and a user profile table
- Each user has one profile

This relationship is relatively rare and often merged into a single table unless separation is necessary.

---

### One-to-Many (1:N)

One row in Table A relates to many rows in Table B.

Example:
- One customer can have many orders
- Each order belongs to one customer

This is the **most common relationship** in relational databases.

Implementation:
- The “many” table contains a foreign key referencing the “one” table

---

### Many-to-Many (M:N)

Rows in Table A can relate to many rows in Table B, and vice versa.

Example:
- Students and courses
- A student can enroll in many courses
- A course can have many students

Implementation:
- Requires a junction (join) table
- The junction table contains foreign keys to both tables

---

## Cardinality and Foreign Keys

Cardinality is enforced through:
- Primary keys
- Foreign keys
- Constraints

These ensure:
- Referential integrity
- Valid relationships between tables
- No orphaned records

---

## How Normalization and Cardinality Work Together

Normalization determines:
- How data is split across tables
- Where each piece of data belongs

Cardinality determines:
- How those tables are connected
- How many records can relate across tables

Together, they form the backbone of relational database design.

---

## Common Design Mistakes

- Storing repeated data instead of normalizing
- Ignoring cardinality and duplicating rows
- Using one table where multiple normalized tables are needed
- Forgetting to enforce relationships with foreign keys

---

## Mental Model

- **Normalization** answers: “Where should this data live?”
- **Cardinality** answers: “How do these pieces of data relate?”

A well-designed database uses normalization to keep data clean and cardinality to keep relationships clear. Both are essential for building systems that scale, remain consistent, and are easy to query over time.
