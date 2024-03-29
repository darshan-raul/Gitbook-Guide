# Base64 encoding

Here's a breakdown of how Base64 encoding works, step by step:

**1. Binary Data Grouping**

* Base64 works on binary data. Your original data (text, image, etc.) is converted into a sequence of bytes (groups of 8 bits).
* These bytes are further grouped into sets of 3 bytes (totaling 24 bits).

**2. Splitting into 6-bit Chunks**

* Each group of 24 bits is divided into four 6-bit chunks.
* This is key because Base64 uses a set of 64 safe characters to represent data, and each 6-bit chunk can represent a value from 0-63.

**3. Mapping to Base64 Characters**

* Here's a standard Base64 character table:
  * `A-Z` (values 0 - 25)
  * `a-z` (values 26 - 51)
  * `0-9` (values 52 - 61)
  * `+` (value 62)
  * `/` (value 63)
* Each 6-bit chunk is used as an index into this table. The corresponding character becomes the Base64 representation of that chunk.

**4. Handling Padding**

* If the original data length isn't divisible by 3, special padding characters (`=`) are added to the end.
  * One `=` indicates the last group had only 8 useful bits.
  * Two `=` signs indicate the last group had only 16 useful bits.

**Example: Encoding "Hi!"**

1. **Binary:** "Hi!" in binary is `01001000 01101001 00100001`
2. **Grouping:** `010010 000110 100100 100001`
3. **Mapping:**
   * `010010` -> `S`
   * `000110` -> `G`
   * `100100` -> `k`
   * `100001` -> `h`
4. **Result:** "SGkh"

**Why Base64?**

* **Safe Transport:** Base64 uses printable ASCII characters, making the encoded data safe to include in emails, URLs, configuration files, etc.
* **Overhead:** Base64 encoding increases the size of the data by about 33% (4 output characters for every 3 input bytes).
