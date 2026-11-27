# SQL, MySQL & PostgreSQL Interview Master Guide

---

# Part 1: Core SQL Foundation (Common to MySQL & PostgreSQL)

---

### Q1. What is SQL and what are CRUD operations?
**Answer:** SQL (Structured Query Language) is the standard language for relational databases. CRUD represents the four basic operations:
- **CREATE**: Creating schemas, tables (`CREATE`), and adding rows (`INSERT`).
- **READ**: Querying data (`SELECT`).
- **UPDATE**: Modifying existing table data (`UPDATE`).
- **DELETE**: Removing data (`DELETE`).

---

### Q2. What is the difference between SQL (Relational) and NoSQL databases?
**Answer:**
| Feature | SQL (RDBMS) | NoSQL |
| :--- | :--- | :--- |
| **Data Model** | Tables with predefined schema | Key-Value, Document (JSON/BSON), Wide-Column, Graph |
| **ACID Compliance** | Strict ACID compliance | Typically follows BASE (Eventual Consistency) |
| **Scaling** | Vertical scaling (scale-up CPU/RAM) | Horizontal scaling (scale-out distributed nodes) |
| **Best For** | Complex joins, transactions, structured financial/operational data | High-throughput, unstructured data, real-time analytics |
| **Examples** | PostgreSQL, MySQL, MariaDB, Oracle | MongoDB, Cassandra, DynamoDB, Redis |

---

### Q3. What are the classifications of SQL commands (DDL, DML, DCL, TCL)?
**Answer:**
1. **DDL (Data Definition Language)**: Defines/modifies schema structure. Auto-commits in MySQL.
   - `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME`
2. **DML (Data Manipulation Language)**: Modifies actual table records.
   - `INSERT`, `UPDATE`, `DELETE`, `MERGE` (`SELECT` is often classified under DQL)
3. **DCL (Data Control Language)**: Manages permissions.
   - `GRANT`, `REVOKE`
4. **TCL (Transaction Control Language)**: Manages transactional boundaries.
   - `COMMIT`, `ROLLBACK`, `SAVEPOINT`

---

### Q4. What is the difference between DELETE, TRUNCATE, and DROP?
**Answer:**
| Criteria | DELETE | TRUNCATE | DROP |
| :--- | :--- | :--- | :--- |
| **Command Type** | DML | DDL | DDL |
| **WHERE Clause** | Yes, can delete specific rows | No, clears the whole table | No, removes the entire table |
| **Rollback** | Yes (inside transaction) | Transaction-dependent (Rollback works in PostgreSQL, but NOT in MySQL) | Rollback works in PostgreSQL transaction, NOT in MySQL |
| **Performance** | Slower (row-by-row log & triggers fire) | Fast (deallocates data pages) | Instant removal of entire table metadata |
| **Triggers** | Fires DELETE triggers | Does NOT fire row-level triggers | Does not fire triggers |

---

### Q5. What is the difference between Primary Key, Unique Key, and Foreign Key?
**Answer:**
- **Primary Key (PK)**: Uniquely identifies each row. Does NOT accept `NULL` values. Only **one** PK per table (automatically creates a clustered index in MySQL InnoDB).
- **Unique Key (UK)**: Ensures distinct values in a column. Accepts `NULL` values (MySQL and PostgreSQL permit multiple `NULL`s in unique columns). A table can have **multiple** unique keys.
- **Foreign Key (FK)**: Enforces referential integrity by referencing the primary key or unique key of another table.

---

### Q6. What is the exact Order of Execution in a SQL Query?
**Answer:** A database engine parses and executes queries in this strict logical sequence:
1. **`FROM` / `JOIN`**: Identify source tables and perform join filtering (`ON`).
2. **`WHERE`**: Filters individual rows before grouping.
3. **`GROUP BY`**: Groups rows matching criteria.
4. **`HAVING`**: Filters grouped rows using aggregate expressions.
5. **`SELECT`**: Evaluates expressions and projects column list.
6. **`DISTINCT`**: Eliminates duplicate rows.
7. **`UNION` / `INTERSECT` / `EXCEPT`**: Performs set operations.
8. **`ORDER BY`**: Sorts the finalized dataset.
9. **`LIMIT` / `OFFSET`** (or `FETCH FIRST`): Restricts total output rows.

---

### Q7. What is the difference between WHERE and HAVING?
**Answer:**
- **`WHERE`**: Filters records **before** grouping or aggregation takes place. Cannot contain aggregate functions (e.g., `WHERE SUM(price) > 100` is invalid).
- **`HAVING`**: Filters groups **after** aggregation has occurred. Specifically designed to filter on aggregate results (`HAVING SUM(price) > 100`).

---

### Q8. Explain all types of SQL Joins.
**Answer:**
- **INNER JOIN**: Returns only rows that have matching keys in both tables.
- **LEFT (OUTER) JOIN**: Returns all rows from the left table, plus matched rows from the right table (unmatched columns return `NULL`).
- **RIGHT (OUTER) JOIN**: Returns all rows from the right table, plus matched rows from the left table.
- **FULL (OUTER) JOIN**: Returns all rows from both tables. (Supported natively in PostgreSQL; simulated using `LEFT JOIN UNION RIGHT JOIN` in MySQL).
- **CROSS JOIN**: Produces the Cartesian product of both tables ($M \times N$ rows).
- **SELF JOIN**: A table joined with itself using aliases (e.g., finding Employee and Manager in one table).

---

### Q9. What is the difference between UNION and UNION ALL?
**Answer:**
- **`UNION`**: Combines result sets of two queries and performs an internal distinct sort to **remove duplicates** (slower).
- **`UNION ALL`**: Combines result sets and **retains all rows including duplicates** without sorting (significantly faster).

---

### Q10. What are Window Functions and how do they differ from GROUP BY?
**Answer:**
- `GROUP BY` collapses multiple rows into a single aggregate summary row.
- `Window Functions` calculate aggregations, ranks, or offsets over a defined subset ("window") of rows **without collapsing rows**, keeping individual row identities intact.
- Defined using the `OVER (PARTITION BY ... ORDER BY ...)` clause.

**Key Types:**
- **Ranking**: `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`, `PERCENT_RANK()`
- **Value/Offset**: `LEAD()`, `LAG()`, `FIRST_VALUE()`, `LAST_VALUE()`
- **Aggregates**: `SUM() OVER()`, `AVG() OVER()`, `COUNT() OVER()`

---

### Q11. What is the difference between ROW_NUMBER(), RANK(), and DENSE_RANK()?
**Answer:** Given a list of identical values (e.g., marks `[90, 80, 80, 70]`):
- **`ROW_NUMBER()`**: Generates consecutive integers with no ties: `1, 2, 3, 4`.
- **`RANK()`**: Assigns identical ranks to ties, then **skips** ranks: `1, 2, 2, 4`.
- **`DENSE_RANK()`**: Assigns identical ranks to ties, but **does not skip** ranks: `1, 2, 2, 3`.

---

### Q12. What is a CTE (Common Table Expression) and Recursive CTE?
**Answer:**
A CTE is a temporary named result set defined using the `WITH` keyword:
```sql
WITH HighEarningEmployees AS (
    SELECT emp_id, salary FROM employee WHERE salary > 100000
)
SELECT * FROM HighEarningEmployees;
```
**Recursive CTE**: References itself to traverse hierarchical or tree-structured data (e.g., organizational charts, category trees):
```sql
WITH RECURSIVE OrgChart AS (
    SELECT emp_id, manager_id, emp_name, 1 AS level FROM employee WHERE manager_id IS NULL
    UNION ALL
    SELECT e.emp_id, e.manager_id, e.emp_name, o.level + 1
    FROM employee e
    JOIN OrgChart o ON e.manager_id = o.emp_id
)
SELECT * FROM OrgChart;
```

---

### Q13. What are ACID properties in Relational Databases?
**Answer:**
- **Atomicity**: An entire transaction executes completely or rolls back entirely (All-or-Nothing).
- **Consistency**: The database transitions from one valid state to another, strictly satisfying all constraints and triggers.
- **Isolation**: Concurrent transactions execute without interfering with one another.
- **Durability**: Once a transaction is committed, its changes survive system crashes or power failures (persisted via WAL / Redo Log).

---

### Q14. What are the 4 Transaction Isolation Levels and their Concurrency Read Phenomena?
**Answer:**
| Isolation Level | Dirty Read | Non-Repeatable Read | Phantom Read |
| :--- | :---: | :---: | :---: |
| **Read Uncommitted** | Possible | Possible | Possible |
| **Read Committed** (Default in PostgreSQL) | **Prevented** | Possible | Possible |
| **Repeatable Read** (Default in MySQL InnoDB) | **Prevented** | **Prevented** | Prevented (via MVCC & Next-Key Locks in InnoDB) |
| **Serializable** | **Prevented** | **Prevented** | **Prevented** |

- **Dirty Read**: Reading uncommitted changes made by another transaction.
- **Non-Repeatable Read**: Re-reading a row within the same transaction produces different values because another transaction modified and committed it.
- **Phantom Read**: Re-running a range query returns newly inserted or deleted rows committed by another transaction.

---

### Q15. How do Database Indexes work, and what is a B-Tree Index?
**Answer:**
An index is an auxiliary data structure that allows fast lookup without scanning every row in a table (avoiding full table scans).
- **B-Tree (Balanced Tree)**: The default index in MySQL and PostgreSQL. Keeps data sorted with balanced tree depth ($O(\log N)$ lookup, insert, and delete complexity).
- Excellent for equality (`=`) and range queries (`<`, `<=`, `>`, `>=`, `BETWEEN`).

---

### Q16. What is the difference between a Clustered Index and a Non-Clustered (Secondary) Index?
**Answer:**
- **Clustered Index**: The actual leaf nodes of the B-Tree contain the entire table row data. The physical order of data on disk matches the index order. A table can only have **one** clustered index (in MySQL InnoDB, this is the Primary Key).
- **Non-Clustered (Secondary) Index**: The leaf nodes contain index keys and a pointer/reference to the actual data row (in MySQL InnoDB, the pointer is the Primary Key value; in PostgreSQL, it's the Heap Tuple ID / TID).

---

# Part 2: MySQL Specific Deep-Dive (Interview Favorites)

---

### Q17. What are Storage Engines in MySQL? Compare InnoDB vs MyISAM.
**Answer:**
MySQL decouples query processing from data storage using pluggable storage engines.
| Feature | InnoDB (Default) | MyISAM (Legacy) |
| :--- | :--- | :--- |
| **Transactions (ACID)** | Fully supported | Not supported |
| **Locking Level** | Row-level locking | Table-level locking |
| **Foreign Keys** | Supported | Not supported |
| **Crash Recovery** | Robust (via Redo Log & Undo Log) | Poor (requires repair table) |
| **Full-Text Search** | Supported (v5.6+) | Supported |

---

### Q18. How does InnoDB achieve high concurrency using MVCC (Multi-Version Concurrency Control)?
**Answer:**
InnoDB implements **MVCC** so that read operations never block write operations, and write operations never block read operations.
- Every row in InnoDB has hidden system columns:
  1. `DB_TRX_ID`: The transaction ID of the last transaction that inserted or updated the row.
  2. `DB_ROLL_PTR`: Roll pointer pointing to the undo log segment holding earlier versions of the row.
- When reading under `Repeatable Read`, a **Read View** is created at the start of the transaction. Queries reconstruct the row state as of that transaction snapshot using the undo log.

---

### Q19. What are Redo Logs, Undo Logs, and Binlogs in MySQL?
**Answer:**
- **Redo Log (InnoDB)**: A crash-recovery disk log (WAL) ensuring durability. Written sequentially before dirty pages are flushed from the buffer pool to data files.
- **Undo Log (InnoDB)**: Stores before-images of modified data. Used to roll back active transactions and build previous row versions for MVCC read consistency.
- **Binlog (Binary Log)**: A server-level log that records all DDL and DML operations. Essential for **MySQL Replication** (Master-Slave/Replica) and Point-In-Time Recovery (PITR).

---

### Q20. What is a Composite Index and the "Leftmost Prefix Rule" in MySQL?
**Answer:**
A composite index covers multiple columns, e.g., `INDEX idx_abc (a, b, c)`.
- **Leftmost Prefix Rule**: The query can utilize the index only if conditions include the leading leftmost column(s):
  - `WHERE a = 1` -> Index **USED**.
  - `WHERE a = 1 AND b = 2` -> Index **USED**.
  - `WHERE a = 1 AND c = 3` -> Index used **partially** (only for `a`).
  - `WHERE b = 2 AND c = 3` -> Index **NOT USED** (missing leading column `a`).

---

### Q21. What is a Covering Index in MySQL?
**Answer:**
An index that contains all the columns required by the query (`SELECT`, `WHERE`, `ORDER BY`).
- When a query is covered, MySQL satisfies the entire query directly from the index B-Tree without performing a secondary lookup on the clustered table records (shown as `Using index` in `EXPLAIN`).

---

### Q22. How do you analyze and optimize slow queries in MySQL using EXPLAIN?
**Answer:**
Run `EXPLAIN SELECT ...` to inspect the execution plan. Key output columns:
- **`type` (Access type, best to worst)**:
  `system` > `const` > `eq_ref` > `ref` > `range` > `index` > `ALL` (full table scan - avoid!).
- **`possible_keys` & `key`**: Indices considered vs index actually chosen.
- **`rows`**: Estimated number of rows MySQL expects to examine.
- **`Extra`**:
  - `Using index`: Excellent (Covering index).
  - `Using filesort`: Needs index on `ORDER BY`.
  - `Using temporary`: Creates temporary memory/disk table (needs query/index optimization).

---

### Q23. What are Record Locks, Gap Locks, and Next-Key Locks in InnoDB?
**Answer:**
- **Record Lock**: Locks the index record itself.
- **Gap Lock**: Locks the empty space ("gap") between index records or before the first/after the last index record. It prevents other transactions from inserting new rows into the gap.
- **Next-Key Lock**: A combination of a **Record Lock** and a **Gap Lock** on the gap preceding the record.
- **Why it matters**: InnoDB uses Next-Key Locking in `Repeatable Read` isolation to eliminate **Phantom Reads**.

---

### Q24. How does Replication work in MySQL?
**Answer:**
1. The **Primary (Master)** writes data changes to its **Binary Log (binlog)**.
2. The **Replica (Slave)** runs an **I/O Thread** that connects to the Primary, reads binlog events, and copies them to the local **Relay Log**.
3. The Replica runs a **SQL Thread** that reads events from the Relay Log and applies them sequentially to the replica's database.

---

# Part 3: PostgreSQL Specific Deep-Dive (Interview Favorites)

---

### Q25. What is the Object-Relational architecture of PostgreSQL?
**Answer:**
PostgreSQL is an **Advanced Object-Relational Database Management System (ORDBMS)**. Unlike traditional RDBMS:
- It supports user-defined custom types, composite types, table inheritance, operator overloading, and foreign data wrappers (FDWs).
- Provides first-class support for unstructured data types (`JSONB`, `XML`, `HSTORE`, `UUID`, arrays, ranges).

---

### Q26. How does MVCC work in PostgreSQL, and why is VACUUM necessary?
**Answer:**
- In PostgreSQL, updates and deletes do not overwrite data in place.
- **UPDATE**: Inserts a brand new tuple (row version) and marks the old tuple as dead by updating its header `xmax`.
- **DELETE**: Marks the tuple as dead with the transaction ID in `xmax`.
- **Dead Tuples & Bloat**: Dead tuples remain on disk to provide consistent read views for older active transactions.
- **VACUUM**: Scans tables to mark dead tuple space as reusable for future inserts.
- **VACUUM FULL**: Rewrites the entire table into a new disk file to reclaim physical operating system disk space (takes an exclusive table lock).
- **AutoVacuum**: A built-in daemon that automatically analyzes and vacuums tables when threshold modifications occur.

---

### Q27. What is the difference between JSON and JSONB in PostgreSQL?
**Answer:**
| Criteria | `JSON` | `JSONB` |
| :--- | :--- | :--- |
| **Storage Format** | Plain text string representation | Decomposed binary format |
| **Insert Speed** | Faster (no binary parsing on insert) | Slightly slower (parsing overhead) |
| **Query Speed** | Slower (must parse text every read) | **Very Fast** (indexed object traversal) |
| **White Space & Key Order** | Preserves exact spaces & duplicate keys | Strips whitespace & normalizes keys |
| **Indexing** | Only functional expression index | **Supports GIN and GIST indexing natively** |

*In almost all production applications, `JSONB` is strongly preferred.*

---

### Q28. What specialized Index Types are available in PostgreSQL?
**Answer:**
PostgreSQL provides 6 built-in index algorithms:
1. **B-Tree**: Default, general purpose for `=`, `<`, `>`, `BETWEEN`.
2. **GIN (Generalized Inverted Index)**: Ideal for multi-valued elements like `JSONB`, full-text search vectors (`tsvector`), and array columns (`int[]`).
3. **GiST (Generalized Search Tree)**: Ideal for geometric, spatial (PostGIS), and range types.
4. **BRIN (Block Range Index)**: Extremely small index for huge, naturally ordered sequential tables (e.g., append-only logs, timestamps, IoT metrics).
5. **Hash**: Fast equality lookups (`=`).
6. **SP-GiST**: Space-partitioned GiST for non-balanced hierarchical datasets (e.g., quadtrees, radix trees).

---

### Q29. What is a Partial Index and an Expression (Functional) Index in PostgreSQL?
**Answer:**
- **Partial Index**: An index built over a subset of rows defined by a `WHERE` clause:
  ```sql
  CREATE INDEX idx_active_users ON users (email) WHERE is_active = true;
  ```
  *Saves disk space and index update overhead for large tables.*
- **Expression Index**: An index built on the result of an expression/function rather than a raw column:
  ```sql
  CREATE INDEX idx_lower_email ON users (LOWER(email));
  ```
  *Speeds up case-insensitive queries like `WHERE LOWER(email) = 'test@example.com'`.*

---

### Q30. What is WAL (Write-Ahead Logging) and how is it used in PostgreSQL Replication?
**Answer:**
- **WAL**: All modifications are written and persisted to WAL segments sequentially on disk **before** actual data page changes are made in memory/heap. Guarantees ACID durability and crash recovery.
- **Physical Streaming Replication**: The primary server streams WAL records byte-for-byte over a network connection to standby replicas.
- **Logical Replication**: Replicates data changes at the table/row level using a Publish/Subscribe model, allowing cross-version replication and selective table streaming.

---

### Q31. What are Common Table Partitioning strategies in PostgreSQL?
**Answer:**
PostgreSQL supports declarative table partitioning:
1. **Range Partitioning**: Partitioned by date ranges or numeric ranges (e.g., `PARTITION BY RANGE (created_at)` for monthly partitions).
2. **List Partitioning**: Partitioned by explicit key values (e.g., `PARTITION BY LIST (country_code)`).
3. **Hash Partitioning**: Distributes rows evenly across partitions using modulus hashing (e.g., `PARTITION BY HASH (user_id)`).

---

### Q32. How do EXPLAIN and EXPLAIN ANALYZE work in PostgreSQL?
**Answer:**
- `EXPLAIN SELECT ...`: Shows the optimizer's estimated plan (estimated cost, startup cost, total cost, row counts) without running the query.
- `EXPLAIN (ANALYZE, BUFFERS) SELECT ...`: **Actually executes** the query and returns exact elapsed wall-clock times, actual row counts, and buffer hits/reads from RAM cache vs disk.

---

# Part 4: Direct Architectural Comparison (MySQL vs PostgreSQL)

---

### Q33. Comprehensive Architectural Differences: MySQL vs PostgreSQL
**Answer:**
| Comparison Angle | MySQL (InnoDB) | PostgreSQL |
| :--- | :--- | :--- |
| **Process Model** | Multi-threaded (single process, multiple threads) | Multi-process (each connection is a dedicated backend process) |
| **Clustered Storage** | Tables are clustered on Primary Key by default | Heap-based tables (tuples stored wherever space exists; unclustered) |
| **MVCC Implementation** | In-place updates + Undo log rollback segments | New tuple written for every update + dead tuples cleaned by `VACUUM` |
| **Data Types & Extensibility** | Basic types; strict SQL adherence | Custom types, composite types, `JSONB`, Arrays, PostGIS extensions |
| **Indexing Power** | B-Tree, Full-Text, Spatial (limited) | B-Tree, GIN, GiST, BRIN, SP-GiST, Hash, Partial, Expression |
| **Default Isolation Level** | `REPEATABLE READ` | `READ COMMITTED` |
| **DDL Transactions** | DDL cannot be rolled back (auto-commits) | **DDL is fully transactional** (`CREATE/DROP TABLE` can be rolled back!) |

---

# Part 5: High-Frequency Interview Coding & Scenario Questions

---

### Q34. How do you find the Nth highest salary in SQL?
**Answer (Method 1: Using DENSE_RANK)**:
```sql
WITH RankedSalaries AS (
    SELECT emp_name, salary,
           DENSE_RANK() OVER (ORDER BY salary DESC) as rank_pos
    FROM employee
)
SELECT emp_name, salary
FROM RankedSalaries
WHERE rank_pos = 2; -- 2nd highest salary
```
**Answer (Method 2: Using LIMIT / OFFSET)**:
```sql
SELECT DISTINCT salary
FROM employee
ORDER BY salary DESC
LIMIT 1 OFFSET 1; -- 2nd highest
```

---

### Q35. How do you find and delete duplicate records from a table while keeping one record?
**Answer (PostgreSQL using `ctid`)**:
```sql
DELETE FROM employee
WHERE ctid NOT IN (
    SELECT MIN(ctid)
    FROM employee
    GROUP BY email
);
```
**Answer (Generic SQL / MySQL using CTE and ROW_NUMBER)**:
```sql
WITH DuplicateCTE AS (
    SELECT id,
           ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) AS rn
    FROM employee
)
DELETE FROM employee
WHERE id IN (
    SELECT id FROM DuplicateCTE WHERE rn > 1
);
```

---

### Q36. What is a Deadlock and how do you prevent or troubleshoot it?
**Answer:**
A deadlock occurs when two or more transactions hold locks that the other transactions need, creating a circular wait cycle:
- **Transaction A**: Holds Lock on Row 1, requests Lock on Row 2.
- **Transaction B**: Holds Lock on Row 2, requests Lock on Row 1.
- **Resolution**:
  - The DB engine's deadlock detector automatically aborts and rolls back the transaction with the lower cost.
- **Prevention Best Practices**:
  1. Always acquire locks and access tables/rows in the **exact same consistent order** across all application services.
  2. Keep transactions short and avoid user interaction inside transactions.
  3. Use appropriate lower isolation levels when possible.
  4. Use `SELECT ... FOR UPDATE SKIP LOCKED` or `NOWAIT` for queue processing.
