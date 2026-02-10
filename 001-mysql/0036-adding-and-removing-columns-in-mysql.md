# Adding and Removing Columns in MySQL

As applications evolve, database schemas must evolve with them. MySQL allows you to **add, remove, and modify columns** in an existing table using the `ALTER TABLE` statement. This is a normal and expected part of database development.

This guide explains how column changes work, when to use them, and the correct syntax, step by step.

---

## The ALTER TABLE Statement

All column-level changes in MySQL are performed using:

    ALTER TABLE table_name
    action;

`ALTER TABLE` modifies the **structure** of a table without deleting the table itself.

---

## Adding a Column

### Basic Syntax

    ALTER TABLE table_name
    ADD column_name data_type;

Example:

    ALTER TABLE customers
    ADD phone_number VARCHAR(20);

This:
- Adds a new column named `phone_number`
- Appends it to the end of the table
- Sets existing rows to `NULL` for this column

---

## Adding a Column with Constraints

    ALTER TABLE customers
    ADD is_active TINYINT NOT NULL DEFAULT 1;

This ensures:
- The column cannot be NULL
- Existing rows get a default value
- New rows automatically inherit the default

---

## Adding Multiple Columns at Once

    ALTER TABLE customers
    ADD address VARCHAR(255),
    ADD city VARCHAR(100),
    ADD country VARCHAR(100);

Multiple columns can be added in a single operation.

---

## Controlling Column Position

By default, MySQL adds new columns at the end. You can control placement.

### Add Column at the Beginning

    ALTER TABLE customers
    ADD created_at DATETIME FIRST;

### Add Column After Another Column

    ALTER TABLE customers
    ADD updated_at DATETIME AFTER created_at;

This affects only column order, not query behavior.

---

## Removing a Column

### Basic Syntax

    ALTER TABLE table_name
    DROP column_name;

Example:

    ALTER TABLE customers
    DROP phone_number;

This:
- Permanently removes the column
- Deletes all data stored in it
- Cannot be undone without backups

---

## Removing Multiple Columns

    ALTER TABLE customers
    DROP address,
    DROP city;

All specified columns are removed in one operation.

---

## Modifying an Existing Column

### Changing Data Type or Constraints

    ALTER TABLE customers
    MODIFY email VARCHAR(255) NOT NULL;

This:
- Changes the column definition
- Keeps the column name
- May fail if existing data violates the new rules

---

## Renaming a Column

### Using CHANGE (Older Style)

    ALTER TABLE customers
    CHANGE email email_address VARCHAR(255);

This requires:
- Old column name
- New column name
- Full column definition

---

### Using RENAME COLUMN (Newer MySQL)

    ALTER TABLE customers
    RENAME COLUMN email TO email_address;

This is cleaner and preferred in modern MySQL versions.

---

## Renaming a Column and Changing Type Together

    ALTER TABLE customers
    CHANGE phone phone_number VARCHAR(30);

This renames and modifies in one step.

---

## Common Pitfalls and Warnings

- Dropping a column deletes data permanently
- Modifying column types can fail due to incompatible data
- Large tables may lock during ALTER operations
- Adding NOT NULL columns without DEFAULT can fail
- Foreign keys and indexes may depend on columns

Always review dependencies before altering tables.

---

## Best Practices for Column Changes

- Always back up production data
- Add columns before removing old ones
- Avoid dropping columns immediately
- Test ALTER statements on staging databases
- Prefer incremental schema changes
- Keep migrations small and reversible

---

## Typical Real-World Workflow

1. Add new column
2. Update application code to use it
3. Backfill data if needed
4. Remove old column later

This minimizes risk and downtime.

---

## Mental Model

Think of `ALTER TABLE` as **reshaping a container**:

- Adding columns adds new compartments
- Removing columns deletes compartments and their contents
- Modifying columns reshapes existing compartments

Schema evolution is normal — mastering `ALTER TABLE` is essential for maintaining real-world MySQL databases.
