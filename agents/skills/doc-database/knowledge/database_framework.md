# Database Architecture, SQL/NoSQL & Performance Framework - Doc Brown

This document details the database engineering standards, indexing strategies, ORM best practices, Redis caching patterns, and ACID transaction rules enforced by **Doc Brown**.

---

## 1. Doc Brown's Database Engineering Directives ("1.21 Gigawatts of Data Efficiency")

1. **SQL & NoSQL Domain Mastery**:
   - **Relational Databases (PostgreSQL, MySQL, SQLite)**: Enforce 3NF normalization, strict foreign key constraints, composite/partial indexing (B-Tree, GIN, GiST), and execution plan analysis (`EXPLAIN ANALYZE`).
   - **NoSQL & Document Stores (MongoDB, DynamoDB)**: Single-table design, document embedding vs. referencing, and partition key strategy.
   - **In-Memory & Caching (Redis)**: Cache-aside pattern, key expiration/TTL strategies, pub/sub, rate-limiting counters, and session storage.
2. **ORM & Query Optimization**:
   - Master ORM mappings (Drizzle, Prisma, TypeORM, Kysely).
   - Prevent the N+1 query problem, over-fetching (select *), and missing indexes on join conditions.
3. **Transactions & ACID Integrity**:
   - Wrap multi-table operations in atomic database transactions (`BEGIN...COMMIT/ROLLBACK`).
   - Prevent deadlocks, race conditions, and dirty reads using appropriate isolation levels.
4. **Zero-Downtime Migrations & Security**:
   - Write reversible, safe database migrations (DDL/DML).
   - Parameterize all SQL queries to eliminate SQL Injection risks completely.
   - Design realistic database seeders for local development and testing environments.

---

## 2. Deliverable Artifact Structure

- `artifacts/database_specification.md`: Full database schema, ERD diagrams, index definitions, ORM models, Redis caching strategy, migration scripts, and seed data specs.
