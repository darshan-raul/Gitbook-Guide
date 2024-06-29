# gitignore

Creating a `.gitignore` file is essential for managing which files and directories Git should ignore in your repository. Here are the basics:

#### 1. **Purpose of `.gitignore`**

* **Prevent accidental commits:** Avoid committing sensitive data, temporary files, or build artifacts.
* **Keep repository clean:** Ensure that only necessary files are tracked by Git.

#### 2. **Syntax**

* Each line in a `.gitignore` file specifies a pattern for files or directories to ignore.
* **Comments:** Lines starting with `#` are comments.
* **Blank lines:** Blank lines are ignored.

#### 3. **Patterns**

* **Wildcards:** Use `*` to match zero or more characters.
  * Example: `*.log` ignores all `.log` files.
* **Directories:** To ignore a directory, include a trailing slash `/`.
  * Example: `build/` ignores the `build` directory.
* **Negation:** Patterns starting with `!` negate a previously ignored pattern.
  * Example: `!important.log` ensures `important.log` is tracked, even if `*.log` is ignored.
* **Relative paths:** Patterns are relative to the location of the `.gitignore` file.



### Cheatsheet

Certainly! Here's a comprehensive `.gitignore` cheatsheet, covering patterns from common to advanced use cases:

### .gitignore Cheatsheet

#### Basic Patterns

1.  **Ignoring Specific Files**

    ```plaintext
    # Ignore all .log files
    *.log
    ```
2.  **Ignoring Directories**

    ```plaintext
    # Ignore a directory and its contents
    mydir/
    ```
3.  **Ignoring Files by Name**

    ```plaintext
    # Ignore all files named 'temp'
    temp
    ```

#### Wildcards

4.  **Single Character Wildcard**

    ```plaintext
    # Ignore all files with a single character followed by 'file.txt'
    ?file.txt
    ```
5.  **Zero or More Characters Wildcard**

    ```plaintext
    # Ignore all .log files in any directory
    **/*.log
    ```

#### Negation

6.  **Negate a Pattern**

    ```plaintext
    # Ignore all .log files except important.log
    *.log
    !important.log
    ```
7.  **Negate a Directory Pattern**

    ```plaintext
    # Ignore all directories named 'mydir' but not 'mydir' at root level
    mydir/
    !/mydir/
    ```

#### Relative Paths

8.  **Relative Path Patterns**

    ```plaintext
    # Ignore file at root level
    /file.txt
    ```
9.  **Ignore Subdirectory Files**

    ```plaintext
    # Ignore all 'file.txt' in any subdirectory of 'mydir'
    mydir/**/file.txt
    ```

#### Advanced Patterns

10. **Composite Patterns**

    ```plaintext
    # Ignore all .log files in any directory but not in 'logs' directory
    **/*.log
    !logs/**/*.log
    ```
11. **Ignoring Files in Subdirectories**

    ```plaintext
    # Ignore all 'config.json' files except in 'src' directory
    config.json
    !src/config.json
    ```
12. **Combining Patterns**

    ```plaintext
    # Ignore all 'temp' files, but not in 'build' and 'dist' directories
    temp
    !build/temp
    !dist/temp
    ```
13. **Complex Negation**

    ```plaintext
    # Ignore all 'bin' files except in the 'tools' directory
    bin/
    !tools/bin
    ```

#### Common Patterns

14. **Operating System Files**

    ```plaintext
    # macOS
    .DS_Store

    # Windows
    Thumbs.db
    desktop.ini
    ```
15. **IDE/Editor Files**

    ```plaintext
    # VS Code
    .vscode/

    # IntelliJ IDEA
    .idea/
    *.iml
    ```
16. **Programming Languages**
    *   **Python**

        ```plaintext
        __pycache__/
        *.pyc
        ```
    *   **Node.js**

        ```plaintext
        node_modules/
        npm-debug.log
        ```
    *   **Java**

        ```plaintext
        *.class
        ```

#### Specific Use Cases

17. **Binary vs Directory**

    ```plaintext
    # Ignore the binary file 'mydir' at the root
    /mydir
    # Do not ignore the directory named 'mydir'
    !/mydir/
    ```
18. **Ignoring Specific File Types in Specific Directories**

    ```plaintext
    # Ignore all .txt files in 'docs' directory
    docs/*.txt
    ```
19. **Ignore Files with Specific Extensions Globally**

    ```plaintext
    # Ignore all .bak files globally
    *.bak
    ```

#### Global .gitignore

20. **Global Ignoring for All Repositories**

    ```sh
    # Create a global .gitignore file
    touch ~/.gitignore_global

    # Add patterns to the global .gitignore
    echo '.DS_Store' >> ~/.gitignore_global

    # Configure Git to use the global .gitignore
    git config --global core.excludesfile ~/.gitignore_global
    ```

###

### &#x20;**Best Practices**

*   **Global `.gitignore`:** For ignoring files globally across all repositories, create a global `.gitignore` file and configure Git to use it:

    ```sh
    git config --global core.excludesfile ~/.gitignore_global
    ```
* **Repository-specific `.gitignore`:** Place the `.gitignore` file in the root of your repository.
* **Review regularly:** Regularly update your `.gitignore` file to reflect changes in your project structure and environment.

#### Example `.gitignore` File

Here is an example of a typical `.gitignore` file for a Node.js project:

```plaintext
# Node.js
node_modules/
npm-debug.log

# Logs
logs/
*.log
yarn-debug.log*
yarn-error.log*

# OS files
.DS_Store
Thumbs.db

# IDE files
.vscode/
.idea/
*.iml
```

By following these basics, you can effectively manage which files and directories are tracked by Git in your repository.



***

### If you have a binary and directory of same name



If you have a directory and a binary inside it with the same name, and you only want to ignore the binary, you can achieve this using a combination of the `.gitignore` patterns.

#### Example Scenario

Let's say you have the following structure:

```
myproject/
├── mydir/
│   └── file.txt
└── mydir  (binary file)
```

#### `.gitignore` Configuration

To ignore only the binary file and not the directory, you can use the following patterns in your `.gitignore` file:

```plaintext
/mydir    # Ignore the binary file named 'mydir'
!/mydir/  # Do not ignore the directory named 'mydir'
```

#### Explanation

* `/mydir` tells Git to ignore the `mydir` file at the root of the repository.
* `!/mydir/` tells Git not to ignore the `mydir` directory and its contents.

#### Full Example

Here's a complete example of what your `.gitignore` file might look like:

```plaintext
# Ignore the binary file named 'mydir'
/mydir

# Do not ignore the directory named 'mydir'
!/mydir/
```

#### Verification

After setting up the `.gitignore` file, you can verify that the binary file is ignored and the directory is not by running the following commands:

1.  **Check the ignored status:**

    ```sh
    git status --ignored
    ```
2.  **List all ignored files:**

    ```sh
    git ls-files --ignored --exclude-standard
    ```



