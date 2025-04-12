# gitattributes

The `.gitattributes` file is used in Git to manage file attributes and define how certain operations are performed on specific files in a repository. Here’s an overview of what it does and how you can use it:

#### Purpose of .gitattributes

1. **Text File Normalization**: Ensures consistent end-of-line handling across different operating systems.
2. **Merge Strategies**: Defines custom merge drivers and strategies for specific files.
3. **Diff Settings**: Customizes how differences are shown for specific files.
4. **Export Settings**: Controls which files are included in archives.
5. **Binary Files**: Specifies binary files to avoid them being treated as text.

#### Basic Syntax

The `.gitattributes` file uses the following format:

```
<pattern> <attribute>
```

#### Common Attributes

1.  **text**: Normalizes line endings.

    * `text=auto`: Detects whether the file is text and normalizes its line endings.
    * `text eol=lf`: Ensures that line endings are LF (Line Feed).
    * `text eol=crlf`: Ensures that line endings are CRLF (Carriage Return Line Feed).



    #### Line Endings in Different Operating Systems

    * **LF (Line Feed, )**: Used by Unix-like systems (Linux, macOS).
    * **CRLF (Carriage Return Line Feed, `\r`)**: Used by Windows.


2. **binary**: Treats the file as binary.
   * `*.jpg binary`: Marks all `.jpg` files as binary.
3. **merge**: Defines custom merge drivers.
   * `*.lock merge=ours`: Uses the "ours" merge strategy for `.lock` files.
4. **diff**: Customizes the diff driver.
   * `*.md diff=markdown`: Uses a custom diff driver for Markdown files.
5. **export-ignore**: Excludes files from archive exports.
   * `*.log export-ignore`: Excludes `.log` files from being included in the archive.
6. **eol**: Specifies the end-of-line character.
   * `*.sh eol=lf`: Ensures that all `.sh` files use LF line endings.

#### Example Usage

Here's an example `.gitattributes` file:

```
# Normalize all text files to use LF
* text=auto

# Ensure Python scripts use LF line endings
*.py eol=lf

# Treat images as binary
*.png binary
*.jpg binary

# Use custom merge strategy for JSON files
*.json merge=json_merge

# Custom diff for Markdown files
*.md diff=markdown

# Ignore log files in export
*.log export-ignore
```

#### Setting Up and Using .gitattributes

1. **Create the File**: Add a `.gitattributes` file at the root of your repository.
2. **Define Patterns and Attributes**: Add patterns and attributes based on your needs.
3. **Commit the File**: Add and commit the `.gitattributes` file to your repository.

```bash
git add .gitattributes
git commit -m "Add .gitattributes for file handling"
```

#### Practical Tips

* **Consistent Line Endings**: Use `.gitattributes` to handle line endings consistently, preventing issues when collaborating across different operating systems.
* **Binary Files**: Mark binary files to prevent Git from attempting to merge them as text, which can corrupt the files.
* **Custom Merge Strategies**: Define custom merge strategies for files that require special handling during merges.
* **Diff Customization**: Enhance diff readability for specific file types with custom diff drivers.

By properly configuring a `.gitattributes` file, you can ensure consistent handling of files in your Git repository, which helps in maintaining code quality and reducing merge conflicts.
