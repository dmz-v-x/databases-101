# How PostgreSQL Works Internally

## Why Understanding PostgreSQL Internals Matters

Many people learn PostgreSQL by:
- Memorizing SQL queries
- Copy-pasting commands
- Treating the database as a black box

This approach works only up to a point.

Problems start when:
- Queries become slow
- Data behaves unexpectedly
- You face production bugs
- Scaling is required

Understanding **how PostgreSQL works internally** gives you:
- Better query-writing instincts
- Clear debugging ability
- Strong performance intuition
- Confidence at scale

---

## What PostgreSQL Is Really Doing Internally

At its core, PostgreSQL is a **long-running server program**.

This means:
- It starts once and keeps running
- It waits for incoming requests
- It processes those requests
- It returns results

PostgreSQL does **not**:
- Wake up only when you write SQL
- Store data magically
- Execute queries directly from text

Every query follows a well-defined internal pipeline.

---

## High-Level Architecture (Bird’s-Eye View)

Internally, PostgreSQL can be understood using three major parts:

1. Client  
2. PostgreSQL Server  
3. Persistent Storage (Disk)

Everything else in PostgreSQL exists to support this flow.

---

## Client Layer

A **client** is any program that talks to PostgreSQL.

Examples:
- `psql` command-line tool
- Backend servers (Node.js, Java, Python, Go)
- GUI tools like pgAdmin

Key idea:

> PostgreSQL itself has no user interface.  
> You always interact with it through a client.

The client:
- Sends SQL queries
- Sends authentication details
- Receives query results

---

## PostgreSQL Server Layer

The PostgreSQL server is the core engine.

It:
- Runs as an operating system process
- Listens on a network port (default: 5432)
- Accepts client connections
- Validates queries
- Executes queries
- Manages concurrency and safety

Think of it as a **database operating system**.

---

## Process-Per-Connection Model

PostgreSQL uses a **process-per-connection** architecture.

What this means:
- Every connected client gets its own backend process
- Queries are isolated from each other
- A crash in one connection does not affect others

This design favors:
- Stability
- Isolation
- Strong reliability

---

## Persistent Storage (Disk)

PostgreSQL stores data permanently on disk.

This includes:
- Table data
- Index data
- System metadata
- Logs

Data is **not stored** as:
- JSON files
- CSV files
- Text files

Instead, PostgreSQL uses its own optimized binary storage format.

You never manually read or edit these files.

---

## End-to-End Query Flow

When you run a query like:

    SELECT * FROM users;

Internally, PostgreSQL follows this flow:

1. Client sends the query
2. Server receives it
3. Server parses and validates it
4. Server plans execution
5. Server executes the plan
6. Results are streamed back to the client

Each step is isolated and intentional.

---

## Query Parsing Phase

Parsing is about **understanding** the SQL.

### Syntax Parsing

PostgreSQL checks:
- SQL grammar
- Keywords
- Structure

If syntax is invalid, execution stops immediately.

---

### Semantic Analysis

PostgreSQL validates meaning:
- Does the table exist?
- Do the columns exist?
- Does the user have permission?

If any rule is violated, PostgreSQL throws an error here.

---

## Query Planner Phase

The **query planner** decides *how* to run the query.

It evaluates:
- Whether to use indexes
- Whether to scan full tables
- Join strategies
- Estimated execution cost

Important rule:

> SQL describes *what* you want,  
> PostgreSQL decides *how* to get it.

The planner chooses the cheapest execution plan.

---

## Query Executor Phase

The executor:
- Follows the chosen execution plan
- Reads data row by row
- Applies filters
- Performs joins
- Aggregates results

It interacts with:
- Memory buffers
- Disk storage
- Index structures

Execution happens incrementally, not all at once.

---

## Result Delivery

As rows are produced:
- They are sent back to the client
- The client displays or processes them

PostgreSQL often streams results instead of waiting for full completion.

---

## Data Directory Structure

PostgreSQL stores everything inside a **data directory**.

Inside it:
- Each database has internal identifiers
- Tables are stored as files
- Indexes are separate files
- System catalogs store metadata

PostgreSQL fully controls this structure.

Manual changes can corrupt the database.

---

## Memory vs Disk

PostgreSQL balances speed and safety using memory and disk together.

Key ideas:
- Disk is slow but persistent
- Memory is fast but temporary
- PostgreSQL aggressively caches data

Major components involved:
- Shared buffers
- OS page cache
- Write-Ahead Log (WAL)

---

## Write-Ahead Logging (High Level)

Write-Ahead Logging ensures durability.

Core idea:
- Changes are written to a log first
- Data files are updated later
- Crashes can be recovered safely

This is the foundation of PostgreSQL’s reliability.

---

## Background Processes

PostgreSQL runs multiple background processes to keep the system healthy:

- WAL writer
- Checkpointer
- Autovacuum
- Statistics collector

These processes:
- Clean up old data
- Flush memory to disk
- Maintain performance
- Prevent data corruption

They run automatically without user involvement.

---

## Core Internal Mental Model

PostgreSQL internally works like a pipeline:

- Clients send intent (SQL)
- Server validates intent
- Planner chooses strategy
- Executor performs work
- Storage ensures durability
- Results flow back

Once this model is clear, advanced topics become logical instead of mysterious.
