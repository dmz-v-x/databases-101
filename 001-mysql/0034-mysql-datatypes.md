# MySQL Data Types 

MySQL data types define **what kind of data a column can store**, how much space it uses, and how MySQL treats that data during comparisons, sorting, and calculations. Choosing the correct data type is critical for correctness, performance, and long-term maintainability.

This guide explains **all commonly used MySQL data types**, why they exist, when to use them, and their trade-offs, step by step.

---

## Why Data Types Matter

Every column in a MySQL table must have a data type. The data type determines:

- What values are allowed
- How much storage is required
- How fast queries run
- How comparisons and calculations behave

Using the wrong data type can lead to:
- Wasted storage
- Slower queries
- Precision loss
- Incorrect results

---

## Integer (Whole Number) Data Types

Integer types are used to store **whole numbers**, both positive and negative.

---

### TINYINT

- Range: `-128` to `127` (signed)
- Uses very little storage

Common use cases:
- Boolean-like values (`0` or `1`)
- Small counters
- Status flags

Example:
    is_active TINYINT

---

### SMALLINT

- Range: `-32,768` to `32,767`

Use when:
- Numbers are small but larger than TINYINT
- Storage efficiency matters

Example:
    stock_quantity SMALLINT

---

### MEDIUMINT

- Range: `-8,388,608` to `8,388,607`

Less commonly used, but helpful for:
- Medium-sized counters
- IDs that do not need INT size

Example:
    views MEDIUMINT

---

### INT (INTEGER)

- Standard integer type
- Very commonly used
- Suitable for most IDs and counters

Example:
    customer_id INT

---

### BIGINT

- Used for very large integers
- Often used for:
  - High-scale systems
  - Large IDs
  - Financial systems

Example:
    transaction_id BIGINT

---

## Decimal and Floating-Point Data Types

These are used for numbers **with decimal points**.

---

### FLOAT

- Approximate decimal values
- Precision up to ~23 digits
- Faster but less accurate

Use when:
- Minor precision loss is acceptable
- Scientific or approximate values

Example:
    temperature FLOAT

---

### DOUBLE

- Approximate decimal values
- Precision between 23 and 53 digits
- More precise than FLOAT

Example:
    latitude DOUBLE

---

### DECIMAL (NUMERIC)

- Exact decimal values
- Precision up to 65 digits
- Best choice for money and financial data

Use when:
- Precision is critical
- No rounding errors are acceptable

Example:
    price DECIMAL(10,2)

---

## String (Text) Data Types

String types store **textual data**.

---

### CHAR

- Fixed-length string
- Length: 0–255 characters
- Always uses the same space

Use when:
- Values are always the same length

Example:
    country_code CHAR(2)

---

### VARCHAR

- Variable-length string
- Length: 0–255 characters (can be higher depending on charset)
- Most commonly used string type

Use when:
- String length varies

Example:
    email VARCHAR(255)

---

### TEXT

- Variable-length string
- Maximum size: 65,535 characters
- Stored differently than VARCHAR

Use when:
- Large text is required

Example:
    description TEXT

---

### Important Correction

There is **no string version of TINYINT**.

The correct small text type is:

### TINYTEXT

- Maximum length: 255 characters
- Similar to VARCHAR(255), but stored differently

Example:
    short_note TINYTEXT

---

## Date and Time Data Types

Used to store **dates and timestamps**.

---

### DATE

- Format: `YYYY-MM-DD`
- Stores only date

Example:
    birth_date DATE

---

### DATETIME

- Format: `YYYY-MM-DD HH:MM:SS`
- Stores date and time
- Time-zone independent

Example:
    created_at DATETIME

---

### TIMESTAMP

- Format: `YYYYMMDDHHMMSS`
- Time-zone aware
- Automatically updated in some cases

Commonly used for:
- Record creation time
- Update tracking

Example:
    updated_at TIMESTAMP

---

## ENUM Data Type

`ENUM` restricts a column to **one value from a predefined list**.

Example:
    status ENUM('active', 'inactive', 'blocked')

Rules:
- Only listed values are allowed
- Stored efficiently
- Easy to misuse if values change frequently

Use with caution.

---

## Choosing the Right Data Type (Guidelines)

- Use `INT` for most IDs
- Use `BIGINT` for large-scale systems
- Use `DECIMAL` for money
- Use `VARCHAR` for most strings
- Use `TEXT` only when strings are large
- Use `DATE` or `DATETIME` instead of strings for dates
- Avoid overusing `ENUM` in evolving systems

---

## Summary Table of Common MySQL Data Types

| Data Type   | Category   | Specification / Range |
|------------|-----------|------------------------|
| TINYINT    | Integer   | -128 to 127 |
| SMALLINT  | Integer   | -32,768 to 32,767 |
| MEDIUMINT | Integer   | -8,388,608 to 8,388,607 |
| INT        | Integer   | Standard integer |
| BIGINT     | Integer   | Very large integer |
| FLOAT      | Decimal   | Approx. 23 digits precision |
| DOUBLE     | Decimal   | Approx. 23–53 digits precision |
| DECIMAL   | Decimal   | Exact, up to 65 digits |
| CHAR       | String    | Fixed-length (0–255) |
| VARCHAR    | String    | Variable-length (0–255+) |
| TINYTEXT  | String    | 0–255 characters |
| TEXT       | String    | 0–65,535 characters |
| DATE       | Date      | YYYY-MM-DD |
| DATETIME  | DateTime  | YYYY-MM-DD HH:MM:SS |
| TIMESTAMP | DateTime  | YYYYMMDDHHMMSS |
| ENUM       | Special   | One value from a fixed list |

---

## Final Mental Model

- Data types define **what kind of data is allowed**
- Smaller, accurate types improve performance
- Correct data types prevent bugs
- Schema design starts with data types

Choosing the right MySQL data type is one of the most important skills for writing reliable and scalable databases.
