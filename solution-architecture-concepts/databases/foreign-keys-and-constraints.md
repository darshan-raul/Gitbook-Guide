# Foreign Keys and Constraints

#### Foreign Keys in PostgreSQL

A foreign key is a field (or collection of fields) in one table that uniquely identifies a row of another table. The table containing the foreign key is called the child table, and the table containing the referenced key is called the parent table.

**Purpose of Foreign Keys**

* **Referential Integrity**: Ensures that a value in the child table corresponds to an existing value in the parent table.
* **Relationship Management**: Establishes and enforces relationships between tables.

**Creating Foreign Keys**

To create a foreign key, you use the `FOREIGN KEY` constraint. Here’s how you can define foreign keys in PostgreSQL:

**Basic Example**

```sql
CREATE TABLE authors (
    author_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE books (
    book_id SERIAL PRIMARY KEY,
    title VARCHAR(100) NOT NULL,
    author_id INT,
    FOREIGN KEY (author_id) REFERENCES authors (author_id)
);
```

In this example:

* The `books` table has a foreign key `author_id` that references `author_id` in the `authors` table.

**Composite Foreign Keys**

A foreign key can also reference multiple columns.

```sql
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT,
    product_id INT,
    order_date DATE NOT NULL,
    UNIQUE (customer_id, product_id, order_date)
);

CREATE TABLE order_details (
    order_detail_id SERIAL PRIMARY KEY,
    order_id INT,
    customer_id INT,
    product_id INT,
    order_date DATE,
    quantity INT,
    FOREIGN KEY (customer_id, product_id, order_date) REFERENCES orders (customer_id, product_id, order_date)
);
```

### **Foreign Key Constraints**

Foreign key constraints ensure that relationships between tables remain consistent.

**CASCADE Options**

When defining a foreign key, you can specify actions that occur when the referenced row in the parent table is updated or deleted.

* **ON DELETE CASCADE**: Automatically deletes rows in the child table when the corresponding row in the parent table is deleted.
* **ON UPDATE CASCADE**: Automatically updates the value of the foreign key in the child table when the corresponding row in the parent table is updated.

```sql
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers (customer_id) ON DELETE CASCADE ON UPDATE CASCADE
);
```

**Other Actions**

* **SET NULL**: Sets the foreign key to `NULL` if the referenced row is deleted or updated.
* **SET DEFAULT**: Sets the foreign key to its default value if the referenced row is deleted or updated.
* **RESTRICT**: Prevents the deletion or update of the referenced row.
* **NO ACTION**: Similar to `RESTRICT`, but the check is deferred until the end of the transaction.

```sql
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers (customer_id) ON DELETE SET NULL ON UPDATE SET DEFAULT
);
```

#### Constraints in PostgreSQL

Constraints are rules enforced on data columns on a table. They ensure the accuracy and reliability of the data in the database. PostgreSQL supports several types of constraints:

1. **NOT NULL Constraint**
2. **UNIQUE Constraint**
3. **PRIMARY KEY Constraint**
4. **FOREIGN KEY Constraint**
5. **CHECK Constraint**
6. **EXCLUSION Constraint**

**1. NOT NULL Constraint**

Ensures that a column cannot have `NULL` values.

```sql
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);
```

**2. UNIQUE Constraint**

Ensures that all values in a column or a set of columns are unique.

```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    email VARCHAR(100) UNIQUE
);
```

**3. PRIMARY KEY Constraint**

A combination of a `NOT NULL` and `UNIQUE` constraint. Uniquely identifies each row in a table.

```sql
CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);
```

**4. FOREIGN KEY Constraint**

Ensures referential integrity between tables.

```sql
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers (customer_id)
);
```

**5. CHECK Constraint**

Ensures that the value in a column meets a specific condition.

```sql
CREATE TABLE accounts (
    account_id SERIAL PRIMARY KEY,
    balance DECIMAL CHECK (balance >= 0)
);
```

**6. EXCLUSION Constraint**

Ensures that if any two rows are compared on the specified columns or expressions using the specified operators, not all of these comparisons will return `TRUE`.

```sql
CREATE TABLE room_reservations (
    room_id INT,
    start_time TIMESTAMP,
    end_time TIMESTAMP,
    EXCLUDE USING gist (room_id WITH =, tstzrange(start_time, end_time) WITH &&)
);
```

#### Summary

* **Foreign Keys**: Enforce relationships between tables and ensure referential integrity. Can have cascading actions like `CASCADE`, `SET NULL`, `SET DEFAULT`, `RESTRICT`, and `NO ACTION`.
* **Constraints**: Ensure data integrity and correctness. Types include `NOT NULL`, `UNIQUE`, `PRIMARY KEY`, `FOREIGN KEY`, `CHECK`, and `EXCLUSION`.

Using these constraints effectively helps maintain the integrity and reliability of the data within your PostgreSQL database.
