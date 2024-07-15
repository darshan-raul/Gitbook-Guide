# sha256sum

The `sha256sum` command is used to compute and verify SHA-256 cryptographic hash values for files. SHA-256 (Secure Hash Algorithm 256-bit) generates a fixed-size 256-bit (32-byte) hash value, which is unique to the input data. Here's how it helps in checking integrity:

#### Generating a SHA-256 Hash

When you run `sha256sum` on a file, it reads the entire file and processes it through the SHA-256 algorithm, producing a hash value. This hash value is a unique digital fingerprint of the file's contents.

#### Checking Integrity

To verify a file's integrity using `sha256sum`, follow these steps:

1.  **Generate the Hash for the Original File:** Before distributing the file, generate its hash:

    ```bash
    sha256sum original_file > original_file.sha256
    ```

    This command creates a file (`original_file.sha256`) containing the SHA-256 hash and the filename.
2. **Distribute the File and Hash:** Distribute both the original file and the `.sha256` file.
3.  **Verify the File:** When the recipient receives the file, they can verify its integrity by comparing the hash of the received file with the provided hash:

    ```bash
    sha256sum -c original_file.sha256
    ```

    This command reads the `.sha256` file and compares the hash inside it with the hash of the received file. If the hashes match, it indicates the file has not been altered.

#### Why SHA-256?

* **Uniqueness:** The likelihood of two different files producing the same SHA-256 hash is extremely low, ensuring the uniqueness of the hash value for a given file.
* **Tamper Detection:** Even a small change in the file's content will produce a completely different hash, making it easy to detect any modifications.

#### Example

1.  **Generate Hash:**

    ```bash
    echo "Hello, world!" > example.txt
    sha256sum example.txt > example.txt.sha256
    ```

    The `example.txt.sha256` file will contain something like:

    ```
    a591a6d40bf420404a011733cfb7b190d62c65bf0bcda32b3444c96d13f238c8  example.txt
    ```
2.  **Verify File:**

    ```bash
    sha256sum -c example.txt.sha256
    ```

    If `example.txt` is unchanged, the output will be:

    ```
    example.txt: OK
    ```

If the file has been altered, the output will indicate a mismatch, and you will know the file's integrity is compromised.
