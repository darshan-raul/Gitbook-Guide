# Columnar Databases

**Fundamental Difference: Data Storage and Retrieval**

* **RDBMS (Row-Oriented):**
  * Stores data row by row. An entire row (representing an object or entity) is stored contiguously on disk.
  * **Example:** A customer record might be stored as: `(ID, Name, Address, Phone, Email)`.
  * **Ideal for:** OLTP (Online Transaction Processing) workloads where you frequently need to fetch or update entire rows/records.
* **Columnar Databases:**
  * Stores data column by column. Values from a single column are stored contiguously on disk.
  * **Example:** Each customer attribute would be a separate column: `(ID, Name, Address, Phone, Email)`. Columns are often split into separate files.
  * **Ideal for:** OLAP (Online Analytical Processing) workloads with heavy aggregations and analysis over large datasets.

**Practical Implications**

1. **Querying and Aggregations:**
   * **RDBMS:**
     * Fast for fetching complete rows/records.
     * Slower for aggregations over a subset of columns (e.g., calculate the average sales amount across all customers) as it may need to read entire rows even if you only need one column.
   * **Columnar:**
     * Can be slower for fetching entire rows, as it needs to fetch data from multiple columns/files.
     * Extremely efficient for aggregations (`SUM`, `AVG`, `MIN`, `MAX`, etc.) on specific columns since only the necessary column data is read.
2. **Compression:**
   * **Columnar:** Achieves higher compression ratios due to the repetitive nature of data within a column (similar data types and values). This leads to less disk space usage and faster I/O.
3. **Updates and Inserts:**
   * **RDBMS:** Generally faster for individual updates or inserts that modify single rows.
   * **Columnar:** Often slower for single record modification as it may involve updating multiple files. More optimized for bulk inserts or appends.

**Use Cases**

**RDBMS (Row-Oriented)**

* **Transaction-heavy systems:** (eCommerce platforms, banking systems, CRMs)
* **Data with lots of relationships and joins:** Normalization in RDBMS promotes data integrity.
* **Frequent updates or inserts of individual records.**

**Columnar Databases**

* **Data Warehousing and Analytics:** Analyzing trends, calculating metrics (sales over time, product performance, user behavior patterns).
* **Time Series Data:** Storing and analyzing sensor data, log events, etc.
* **Large Datasets:** Where storage efficiency and fast aggregation performance matter more than frequent individual record updates.

**Examples of Columnar Databases**

* ClickHouse
* Apache Cassandra (wide column store)
* Vertica
* Druid
* BigQuery
* AWS Redshift

