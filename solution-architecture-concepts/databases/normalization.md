# Normalization

Database normalization is the process of organizing data in a database to reduce redundancy and improve data integrity. The primary levels of normalization are known as Normal Forms (NFs). Here are the main Normal Forms along with their definitions and examples:

#### 1. First Normal Form (1NF)

**Definition**: A table is in 1NF if:

* It only contains atomic (indivisible) values.
* Each column contains values of a single type.
* Each column has a unique name.
* The order in which data is stored does not matter.

**Example**: Consider a table of students with subjects they are enrolled in.

**Not in 1NF:**

| StudentID | StudentName | Subjects         |
| --------- | ----------- | ---------------- |
| 1         | Alice       | Math, English    |
| 2         | Bob         | Science, History |

**In 1NF:**

| StudentID | StudentName | Subject |
| --------- | ----------- | ------- |
| 1         | Alice       | Math    |
| 1         | Alice       | English |
| 2         | Bob         | Science |
| 2         | Bob         | History |

#### 2. Second Normal Form (2NF)

**Definition**: A table is in 2NF if:

* It is in 1NF.
* All non-key attributes are fully functional dependent on the primary key (no partial dependency).

**Example**: Consider a table of student enrollments with a composite primary key.

**Not in 2NF:**

| StudentID | CourseID | StudentName | CourseName |
| --------- | -------- | ----------- | ---------- |
| 1         | 101      | Alice       | Math       |
| 1         | 102      | Alice       | English    |
| 2         | 101      | Bob         | Math       |
| 2         | 103      | Bob         | History    |

**In 2NF:**

Separate into two tables: **Students**

| StudentID | StudentName |
| --------- | ----------- |
| 1         | Alice       |
| 2         | Bob         |

**Courses**

| CourseID | CourseName |
| -------- | ---------- |
| 101      | Math       |
| 102      | English    |
| 103      | History    |

**Enrollments**

| StudentID | CourseID |
| --------- | -------- |
| 1         | 101      |
| 1         | 102      |
| 2         | 101      |
| 2         | 103      |

#### 3. Third Normal Form (3NF)

**Definition**: A table is in 3NF if:

* It is in 2NF.
* All the attributes are functionally dependent only on the primary key (no transitive dependency).

**Example**: Consider a table with student information and department details.

**Not in 3NF:**

| StudentID | StudentName | DeptID | DeptName     |
| --------- | ----------- | ------ | ------------ |
| 1         | Alice       | 10     | Computer Sci |
| 2         | Bob         | 20     | Physics      |

**In 3NF:**

Separate into two tables: **Students**

| StudentID | StudentName | DeptID |
| --------- | ----------- | ------ |
| 1         | Alice       | 10     |
| 2         | Bob         | 20     |

**Departments**

| DeptID | DeptName     |
| ------ | ------------ |
| 10     | Computer Sci |
| 20     | Physics      |

#### 4. Boyce-Codd Normal Form (BCNF)

**Definition**: A table is in BCNF if:

* It is in 3NF.
* For every non-trivial functional dependency X -> Y, X is a super key.

**Example**: Consider a table where an instructor can only teach one subject.

**Not in BCNF:**

| InstructorID | SubjectID | InstructorName | SubjectName |
| ------------ | --------- | -------------- | ----------- |
| 1            | 101       | John           | Math        |
| 2            | 102       | Alice          | English     |
| 1            | 103       | John           | History     |

**In BCNF:**

Separate into two tables: **Instructors**

| InstructorID | InstructorName |
| ------------ | -------------- |
| 1            | John           |
| 2            | Alice          |

**Subjects**

| SubjectID | SubjectName | InstructorID |
| --------- | ----------- | ------------ |
| 101       | Math        | 1            |
| 102       | English     | 2            |
| 103       | History     | 1            |

#### 5. Fourth Normal Form (4NF)

**Definition**: A table is in 4NF if:

* It is in BCNF.
* It has no multi-valued dependencies.

**Example**: Consider a table where a student can have multiple hobbies and multiple phone numbers.

**Not in 4NF:**

| StudentID | Hobby    | PhoneNumber |
| --------- | -------- | ----------- |
| 1         | Painting | 1234567890  |
| 1         | Singing  | 1234567890  |
| 1         | Painting | 0987654321  |

**In 4NF:**

Separate into two tables: **StudentHobbies**

| StudentID | Hobby    |
| --------- | -------- |
| 1         | Painting |
| 1         | Singing  |

**StudentPhones**

| StudentID | PhoneNumber |
| --------- | ----------- |
| 1         | 1234567890  |
| 1         | 0987654321  |

#### 6. Fifth Normal Form (5NF)

**Definition**: A table is in 5NF if:

* It is in 4NF.
* It cannot be decomposed into smaller tables without losing data (no join dependency).

**Example**: Consider a table where projects have multiple tasks and each task can be handled by multiple teams.

**Not in 5NF:**

| ProjectID | TaskID | TeamID |
| --------- | ------ | ------ |
| 1         | T1     | A      |
| 1         | T1     | B      |
| 1         | T2     | A      |

**In 5NF:**

Separate into three tables: **ProjectsTasks**

| ProjectID | TaskID |
| --------- | ------ |
| 1         | T1     |
| 1         | T2     |

**TasksTeams**

| TaskID | TeamID |
| ------ | ------ |
| T1     | A      |
| T1     | B      |
| T2     | A      |

**ProjectsTeams**

| ProjectID | TeamID |
| --------- | ------ |
| 1         | A      |
| 1         | B      |

