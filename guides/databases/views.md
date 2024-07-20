# Views

A view in PostgreSQL is a virtual table based on the result of an SQL query. Views are used to simplify complex queries, enhance security, and provide a level of abstraction over raw data tables.

#### Key Concepts

1.  **Definition and Creation:**

    * A view is defined by a `SELECT` query.
    * It does not store data physically but displays data from one or more tables.
    * Created using the `CREATE VIEW` statement.

    **Syntax:**

    ```sql
    CREATE VIEW view_name AS
    SELECT column1, column2, ...
    FROM table_name
    WHERE condition;
    ```

    **Example:**

    ```sql
    CREATE VIEW employee_view AS
    SELECT id, first_name, last_name, department
    FROM employees
    WHERE active = true;
    ```
2.  **Using Views:**

    * Querying a view is similar to querying a table.
    * You can use `SELECT` statements to retrieve data from a view.

    **Example:**

    ```sql
    SELECT * FROM employee_view;
    ```
3.  **Updating Views:**

    * Simple views can sometimes be updated directly.
    * Complex views involving joins or aggregations typically cannot be updated directly.

    **Example of an Updatable View:**

    ```sql
    CREATE VIEW simple_view AS
    SELECT id, first_name, last_name
    FROM employees
    WHERE department = 'Sales';

    UPDATE simple_view
    SET first_name = 'John'
    WHERE id = 1;
    ```
4.  **Materialized Views:**

    * Unlike regular views, materialized views store the result set of the query physically.
    * They can be refreshed to update the data.

    **Creating a Materialized View:**

    ```sql
    CREATE MATERIALIZED VIEW mat_view_name AS
    SELECT column1, column2, ...
    FROM table_name
    WHERE condition;
    ```

    **Refreshing a Materialized View:**

    ```sql
    REFRESH MATERIALIZED VIEW mat_view_name;
    ```
5.  **Dropping Views:**

    * Views can be dropped using the `DROP VIEW` statement.

    **Syntax:**

    ```sql
    DROP VIEW view_name;
    ```

    **Example:**

    ```sql
    DROP VIEW employee_view;
    ```

#### Benefits of Using Views

1. **Simplification:**
   * Simplifies complex queries by encapsulating them into a view.
2. **Security:**
   * Restricts access to specific columns or rows in a table by exposing only what is necessary.
3. **Reusability:**
   * Promotes reusability of SQL queries across different applications and users.
4. **Consistency:**
   * Ensures consistent results by standardizing complex queries.

#### Practical Examples

1.  **Join View:**

    ```sql
    CREATE VIEW department_employees AS
    SELECT e.id, e.first_name, e.last_name, d.department_name
    FROM employees e
    JOIN departments d ON e.department_id = d.id;
    ```
2.  **Aggregated View:**

    ```sql
    CREATE VIEW department_summary AS
    SELECT department_id, COUNT(*) AS num_employees, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id;
    ```
3.  **Conditional View:**

    ```sql
    CREATE VIEW active_employees AS
    SELECT id, first_name, last_name
    FROM employees
    WHERE active = true;
    ```

####
