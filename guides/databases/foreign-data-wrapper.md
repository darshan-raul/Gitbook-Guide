# Foreign data wrapper

#### PostgreSQL Foreign Data Wrapper (FDW)

**Foreign Data Wrapper (FDW)** is a PostgreSQL feature that allows you to access data stored in external databases and other data sources as if they were tables within your PostgreSQL database. This enables seamless integration and querying of remote data.

**Key Features of FDW:**

* **Seamless Integration**: Access and query foreign tables using standard SQL.
* **Data Federation**: Combine data from multiple sources into a single query.
* **Performance**: Efficiently handles large datasets with pushdown capabilities, minimizing data transfer.

#### Setting Up PostgreSQL FDW

**Step 1: Install the FDW Extension**

First, you need to install the FDW extension for the specific data source. For PostgreSQL-to-PostgreSQL connections, use the `postgres_fdw` extension.

For **Debian/Ubuntu**:

```sh
sudo apt-get install postgresql-<version>-postgres-fdw
```

For **CentOS/RHEL**:

```sh
sudo yum install postgresql<version>-contrib
```

**Step 2: Load the Extension**

Load the `postgres_fdw` extension into your database:

```sql
CREATE EXTENSION postgres_fdw;
```

**Step 3: Create a Foreign Server**

Define the foreign server that you want to connect to:

```sql
CREATE SERVER foreign_server
FOREIGN DATA WRAPPER postgres_fdw
OPTIONS (host 'remote_host', dbname 'remote_db', port '5432');
```

* **host**: The address of the remote PostgreSQL server.
* **dbname**: The name of the database on the remote server.
* **port**: The port on which the remote PostgreSQL server is running.

**Step 4: Create a User Mapping**

Create a user mapping to provide authentication details for connecting to the foreign server:

```sql
CREATE USER MAPPING FOR local_user
SERVER foreign_server
OPTIONS (user 'remote_user', password 'remote_password');
```

* **local\_user**: The local PostgreSQL user.
* **remote\_user**: The remote PostgreSQL user.
* **remote\_password**: The password for the remote PostgreSQL user.

**Step 5: Import Foreign Schema or Tables**

You can either import the entire schema or specific tables from the foreign server.

To import all tables in a schema:

```sql
IMPORT FOREIGN SCHEMA public
FROM SERVER foreign_server
INTO local_schema;
```

To create a foreign table manually:

```sql
CREATE FOREIGN TABLE foreign_table (
    id integer,
    data text
)
SERVER foreign_server
OPTIONS (schema_name 'public', table_name 'remote_table');
```

* **foreign\_table**: The name of the foreign table in the local database.
* **remote\_table**: The name of the table on the remote server.

#### Querying Foreign Tables

Once the foreign tables are set up, you can query them just like any other local tables:

```sql
SELECT * FROM local_schema.foreign_table;
```

#### Advanced Configuration and Performance Tuning

* **Pushdown Capabilities**: PostgreSQL FDW can push down WHERE clauses, LIMIT clauses, and aggregate functions to the foreign server, reducing the amount of data transferred.
* **Join Pushdown**: PostgreSQL 11 and later support join pushdown, allowing joins to be executed on the foreign server.
* **Performance Tuning**: Use `ANALYZE` on foreign tables to gather statistics, improving query planner decisions.

```sql
ANALYZE local_schema.foreign_table;
```

#### Summary

PostgreSQL FDW provides a powerful way to integrate and query remote data sources seamlessly. By following the setup steps, you can define foreign servers, create user mappings, import foreign schemas or tables, and efficiently query remote data. Advanced configuration options like pushdown capabilities and performance tuning further enhance the usability and performance of FDW.
