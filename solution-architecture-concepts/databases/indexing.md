# Indexing

{% embed url="https://youtu.be/5t1fW3KG920?si=YCYOL_LecLzrpET5" %}

Database indexing is a technique used to improve the speed of data retrieval operations on a database table. An index is a data structure, typically a B-tree or hash table, that allows for fast lookups, insertions, and deletions. Indexes are used to quickly locate data without having to search every row in a database table each time a database table is accessed.

#### Benefits of Indexing

1. **Improved Query Performance**: Indexes can significantly speed up the performance of SELECT queries.
2. **Faster Sorting**: Indexes can speed up the retrieval of ordered data.
3. **Efficient Searching**: Indexes make it possible to retrieve data more efficiently by reducing the number of disk I/O operations.

#### Types of Indexes

1. **Primary Index**: Automatically created when a primary key is defined.
2. **Unique Index**: Ensures all the values in the indexed column are unique.
3. **Composite Index**: An index on multiple columns of a table.
4. **Full-Text Index**: Supports full-text search queries.
5. **Spatial Index**: Used for spatial data types in MySQL (geometry data types).

#### Creating Indexes in MySQL

**Example 1: Creating a Primary Index**

When you define a primary key in a table, MySQL automatically creates a primary index on that column.

```sql
CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    student_name VARCHAR(100),
    age INT,
    major VARCHAR(100)
);
```

**Example 2: Creating a Unique Index**

To ensure that a column has unique values, you can create a unique index.

```sql
CREATE TABLE employees (
    employee_id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(100) UNIQUE,
    first_name VARCHAR(100),
    last_name VARCHAR(100)
);
```

Alternatively, you can add a unique index to an existing column.

```sql
CREATE UNIQUE INDEX idx_unique_email ON employees(email);
```

**Example 3: Creating a Composite Index**

Composite indexes are used to speed up queries that involve multiple columns.

```sql
CREATE TABLE orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    amount DECIMAL(10, 2)
);

CREATE INDEX idx_customer_order ON orders(customer_id, order_date);
```

**Example 4: Creating a Full-Text Index**

Full-text indexes are used for full-text searches on columns that contain large texts.

```sql
CREATE TABLE articles (
    article_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(200),
    body TEXT,
    FULLTEXT (title, body)
);

-- Or add a full-text index to an existing table
CREATE FULLTEXT INDEX idx_fulltext_title_body ON articles(title, body);
```

#### Using Indexes in Queries

**Example 1: Query Using a Primary Key**

The primary key index speeds up this query by allowing MySQL to quickly find the row with `student_id = 1`.

```sql
SELECT * FROM students WHERE student_id = 1;
```

**Example 2: Query Using a Composite Index**

The composite index on `customer_id` and `order_date` makes this query more efficient.

```sql
SELECT * FROM orders WHERE customer_id = 123 AND order_date = '2023-06-20';
```

**Example 3: Query Using a Full-Text Index**

Full-text indexes allow for efficient full-text searches.

```sql
SELECT * FROM articles
WHERE MATCH(title, body) AGAINST ('database indexing');
```

#### Maintenance and Drawbacks

* **Maintenance Overhead**: Indexes need to be maintained, which can add overhead during insert, update, and delete operations.
* **Storage Space**: Indexes consume additional disk space.
* **Index Selection**: Poorly chosen indexes can degrade performance. Careful selection and management are required to balance query performance and maintenance overhead.

#### Summary

Indexing is a powerful tool in database management that significantly improves query performance by reducing the amount of data MySQL needs to scan. However, it's essential to use indexes judiciously, as they can introduce additional storage requirements and maintenance overhead.
