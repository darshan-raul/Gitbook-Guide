# Redirection Commands

Absolutely! Here's a cheatsheet for Linux redirection semantics:

**Basic Redirection**

| Operator | Description                                              | Example                                            |
| -------- | -------------------------------------------------------- | -------------------------------------------------- |
| `>`      | Redirect standard output (stdout) to a file, overwriting | `ls > file.txt`                                    |
| `>>`     | Append standard output (stdout) to a file                | `echo "Hello" >> file.txt`                         |
| `<`      | Redirect standard input (stdin) from a file              | `wc -l < file.txt`                                 |
| `2>`     | Redirect standard error (stderr) to a file, overwriting  | `grep invalid file.txt 2> errors.txt`              |
| `2>>`    | Append standard error (stderr) to a file                 | `find / -name "missingfile" 2>> search_errors.log` |

**Special File Descriptors**

* **0:** Standard input (stdin)
* **1:** Standard output (stdout)
* **2:** Standard error (stderr)

**Combining Standard Output and Error**

| Operator | Description                                            | Example                                                            |
| -------- | ------------------------------------------------------ | ------------------------------------------------------------------ |
| `&>`     | Redirect both stdout and stderr to a file, overwriting | `command &> output.txt`                                            |
| \`       | tee\`                                                  | Redirect stdout to a file while also displaying it in the terminal |
| `2>&1`   | Redirect stderr to the same destination as stdout      | `command 2>&1 output.txt` or `command > output.txt 2>&1`           |

**Advanced Redirection**

*   **Process Substitution:**

    * `<(command)`: Replace with a filename containing the output of the command.
    * `>(command)`: Replace with a filename where input for the command can be written.

    Example: `diff <(ls dir1) <(ls dir2)`
*   **Here Documents (Heredocs):**

    * `<<DELIMITER`: Read input until a line containing only the `DELIMITER` is encountered.

    Example:

    Bash

    ```
    cat <<EOF
    This is a multiline input
    that will be passed to the 'cat' command.
    EOF
    ```
*   **Here Strings (Herestrings):**

    * `<<<"string"`: Treats the quoted string as standard input.

    Example: `wc -w <<<"This is a test"`

**Key Points to Remember**

* Order matters: Redirections are processed in the order they appear on the command line.
* Overwriting: Be cautious with overwriting files, as it erases existing content.
* Combining redirects: You can chain multiple redirections together.
* Error handling: Redirecting stderr is important for capturing error messages.

