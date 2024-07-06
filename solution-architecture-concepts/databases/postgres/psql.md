# psql

Sure, here is a comprehensive cheat sheet for the PostgreSQL command-line interface (`psql`). This should cover most common scenarios you might encounter while working with PostgreSQL.

#### Connecting to PostgreSQL

*   **Connect to a database**

    ```bash
    psql -h hostname -p port -U username -d database
    ```
*   **Connect to a database with a URL**

    ```bash
    psql postgresql://username:password@hostname:port/database
    ```
*   **Switch to a different database**

    ```sql
    \c database_name
    ```

#### psql Meta-Commands

*   **List all databases**

    ```sql
    \l or \list
    ```
*   **List all tables in the current database**

    ```sql
    \dt
    ```
*   **Describe a table**

    ```sql
    \d table_name
    ```
*   **List all indexes**

    ```sql
    \di
    ```
*   **List all sequences**

    ```sql
    \ds
    ```
*   **List all views**

    ```sql
    \dv
    ```
*   **List all functions**

    ```sql
    \df
    ```
*   **List all roles**

    ```sql
    \du
    ```
*   **List current connections**

    ```sql
    \conninfo
    ```
*   **List all schemas**

    ```sql
    \dn
    ```
*   **Show current user**

    ```sql
    \conninfo
    ```

#### SQL Commands

*   **Create a new database**

    ```sql
    CREATE DATABASE database_name;
    ```
*   **Drop a database**

    ```sql
    DROP DATABASE database_name;
    ```
*   **Create a new table**

    ```sql
    CREATE TABLE table_name (
        column_name1 data_type constraints,
        column_name2 data_type constraints,
        ...
    );
    ```
*   **Drop a table**

    ```sql
    DROP TABLE table_name;
    ```
*   **Insert data into a table**

    ```sql
    INSERT INTO table_name (column1, column2, ...)
    VALUES (value1, value2, ...);
    ```
*   **Update data in a table**

    ```sql
    UPDATE table_name
    SET column1 = value1, column2 = value2, ...
    WHERE condition;
    ```
*   **Delete data from a table**

    ```sql
    DELETE FROM table_name
    WHERE condition;
    ```
*   **Select data from a table**

    ```sql
    SELECT column1, column2, ...
    FROM table_name
    WHERE condition;
    ```

#### User and Role Management

*   **Create a new user**

    ```sql
    CREATE USER username WITH PASSWORD 'password';
    ```
*   **Drop a user**

    ```sql
    DROP USER username;
    ```
*   **Grant privileges**

    ```sql
    GRANT privilege ON object TO user;
    ```
*   **Revoke privileges**

    ```sql
    REVOKE privilege ON object FROM user;
    ```

#### Backup and Restore

*   **Backup a database**

    ```bash
    pg_dump -h hostname -U username -d database -F c -b -v -f backup_file
    ```
*   **Restore a database**

    ```bash
    pg_restore -h hostname -U username -d database -v backup_file
    ```

#### Transactions

*   **Start a transaction**

    ```sql
    BEGIN;
    ```
*   **Commit a transaction**

    ```sql
    COMMIT;
    ```
*   **Rollback a transaction**

    ```sql
    ROLLBACK;
    ```

#### System Information

*   **Show server version**

    ```sql
    SELECT version();
    ```
*   **Show current database**

    ```sql
    SELECT current_database();
    ```
*   **Show current user**

    ```sql
    SELECT current_user;
    ```
*   **Show current date and time**

    ```sql
    SELECT now();
    ```

#### Meta-Command Shortcuts

*   **Quit psql**

    ```sql
    \q
    ```
*   **Clear the screen**

    ```sql
    \! clear
    ```
*   **Execute shell command**

    ```sql
    \! command
    ```
*   **Get help on SQL commands**

    ```sql
    \h or \help
    ```
*   **Get help on psql commands**

    ```sql
    \?
    ```

#### Miscellaneous

*   **Enable timing of queries**

    ```sql
    \timing
    ```
*   **Show query execution plan**

    ```sql
    EXPLAIN query;
    ```
*   **Show detailed query execution plan**

    ```sql
    EXPLAIN ANALYZE query;
    ```
*   **Set environment variable**

    ```bash
    export PGUSER=username
    export PGPASSWORD=password
    ```

This cheat sheet should help you navigate and use `psql` effectively for most scenarios you might encounter while working with PostgreSQL.
