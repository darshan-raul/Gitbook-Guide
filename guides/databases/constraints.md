# Constraints

In SQL, there are several options for handling referential actions on `FOREIGN KEY` constraints. These actions dictate what happens to the child table records when the corresponding record in the parent table is updated or deleted. Here are the common options:

#### Referential Actions

1. **CASCADE**:
   * **ON DELETE CASCADE**: Deletes the child rows when the parent row is deleted.
   * **ON UPDATE CASCADE**: Updates the child rows to match the new parent key when the parent key is updated.
2. **SET NULL**:
   * **ON DELETE SET NULL**: Sets the foreign key column in the child table to `NULL` when the parent row is deleted.
   * **ON UPDATE SET NULL**: Sets the foreign key column in the child table to `NULL` when the parent key is updated.
3. **SET DEFAULT**:
   * **ON DELETE SET DEFAULT**: Sets the foreign key column in the child table to its default value when the parent row is deleted.
   * **ON UPDATE SET DEFAULT**: Sets the foreign key column in the child table to its default value when the parent key is updated.
4. **RESTRICT**:
   * **ON DELETE RESTRICT**: Prevents deletion of the parent row if there are any child rows referencing it.
   * **ON UPDATE RESTRICT**: Prevents updating the parent key if there are any child rows referencing it.
5. **NO ACTION**:
   * **ON DELETE NO ACTION**: Similar to RESTRICT, but the check is deferred until the end of the transaction.
   * **ON UPDATE NO ACTION**: Similar to RESTRICT, but the check is deferred until the end of the transaction.

#### Example Table Definitions with Different Constraints

**CASCADE**

```sql
CREATE TABLE projects (
  id SERIAL PRIMARY KEY,
  name VARCHAR,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);

CREATE TABLE stacks (
  id SERIAL PRIMARY KEY,
  name VARCHAR,
  project_id INTEGER REFERENCES projects (id) ON DELETE CASCADE ON UPDATE CASCADE
);
```

**SET NULL**

```sql
CREATE TABLE projects (
  id SERIAL PRIMARY KEY,
  name VARCHAR,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);

CREATE TABLE stacks (
  id SERIAL PRIMARY KEY,
  name VARCHAR,
  project_id INTEGER REFERENCES projects (id) ON DELETE SET NULL ON UPDATE SET NULL
);
```

**SET DEFAULT**

```sql
CREATE TABLE projects (
  id SERIAL PRIMARY KEY,
  name VARCHAR,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);

CREATE TABLE stacks (
  id SERIAL PRIMARY KEY,
  name VARCHAR,
  project_id INTEGER DEFAULT 0 REFERENCES projects (id) ON DELETE SET DEFAULT ON UPDATE SET DEFAULT
);
```

**RESTRICT**

```sql
CREATE TABLE projects (
  id SERIAL PRIMARY KEY,
  name VARCHAR,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);

CREATE TABLE stacks (
  id SERIAL PRIMARY KEY,
  name VARCHAR,
  project_id INTEGER REFERENCES projects (id) ON DELETE RESTRICT ON UPDATE RESTRICT
);
```

**NO ACTION**

```sql
CREATE TABLE projects (
  id SERIAL PRIMARY KEY,
  name VARCHAR,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);

CREATE TABLE stacks (
  id SERIAL PRIMARY KEY,
  name VARCHAR,
  project_id INTEGER REFERENCES projects (id) ON DELETE NO ACTION ON UPDATE NO ACTION
);
```

#### Summary

* **CASCADE**: Automatically propagate changes from parent to child.
* **SET NULL**: Set child foreign key to `NULL` on parent change.
* **SET DEFAULT**: Set child foreign key to its default value on parent change.
* **RESTRICT**: Prevent parent change if child rows exist.
* **NO ACTION**: Defer the action until the end of the transaction.

Choosing the right constraint depends on the specific requirements of your application's data integrity and business logic.
