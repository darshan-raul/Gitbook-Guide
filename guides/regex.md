# Regex

PCRE is a widely used regular expression engine inspired by the syntax of Perl. You'll find it used in many programming languages (PHP, Python's `re` module with some differences, R), web servers (Apache, Nginx), text editors, and command-line tools.

**PCRE Syntax Walkthrough**

1. **Literal Characters:**
   * Most characters match themselves. For example, the regex `cat` matches the literal string "cat".
2. **Metacharacters (Characters with Special Meaning):**
   * `.` (Dot): Matches any single character except newline (unless the 's' modifier is used).
   * `\` (Backslash): Escapes a metacharacter, turning it into a literal character (e.g., `\.` matches a literal dot). It also forms special sequences (see below).
   * `^`: Matches the start of the string (or the start of a line in multiline mode 'm').
   * `$`: Matches the end of the string (or the end of a line in multiline mode 'm').
   * `|` (Pipe): Acts as an OR operator. `cat|dog` matches either "cat" or "dog". (This is key for your multi-keyword question!)
   * `[...]` (Character Set): Matches any single character within the brackets. `[aeiou]` matches any vowel. `[a-zA-Z0-9_]` matches any letter, digit, or underscore.
   * `[^...]` (Negated Character Set): Matches any single character _not_ within the brackets after the caret. `[^0-9]` matches any non-digit character.
   * `(...)` (Grouping): Groups parts of the pattern together.
     * Applies quantifiers (see below) to the whole group: `(abc)+` matches "abc", "abcabc", etc.
     * Captures the matched text for later use (backreferences like `\1`, `\2` or extraction in code).
   * `(?:...)` (Non-Capturing Group): Groups like `(...)` but doesn't capture the match. More efficient if you don't need the captured value.
3. **Quantifiers (Specify Repetition):** Applied to the preceding character, group, or character class.
   * `*`: Matches 0 or more times. `a*` matches "", "a", "aa", "aaa", etc.
   * `+`: Matches 1 or more times. `a+` matches "a", "aa", "aaa", etc. (but not "").
   * `?`: Matches 0 or 1 time (makes the preceding element optional). `colou?r` matches "color" and "colour".
   * `{n}`: Matches exactly _n_ times. `a{3}` matches "aaa".
   * `{n,}`: Matches at least _n_ times. `a{2,}` matches "aa", "aaa", etc.
   * `{n,m}`: Matches between _n_ and _m_ times (inclusive). `a{2,4}` matches "aa", "aaa", "aaaa".
   * **Greedy vs. Lazy:** By default, quantifiers are _greedy_ (they match as much text as possible). Add a `?` after the quantifier to make it _lazy_ (match as little text as possible).
     * `.*` applied to `<p>one</p><p>two</p>` matches `<p>one</p><p>two</p>` (greedy).
     * `.*?` applied to the same string matches `<p>one</p>` first, then `<p>two</p>` on subsequent matches (lazy).
4. **Special Character Sequences (using `\`):**
   * `\d`: Matches any digit character (equivalent to `[0-9]`).
   * `\D`: Matches any non-digit character (equivalent to `[^0-9]`).
   * `\w`: Matches any "word" character<sup>1</sup> (letters, digits, underscore; equivalent to `[a-zA-Z0-9_]`).
   * `\W`: Matches any non-"word" character (equivalent to `[^a-zA-Z0-9_]`).
   * `\s`: Matches any whitespace character (space,<sup>2</sup> tab, newline, carriage return, form feed, vertical tab).
   * `\S`: Matches any non-whitespace character.
   * `\b`: Matches a word boundary (the position between a `\w` and a `\W` character, or start/end of string if the first/last character is a `\w`). Very useful for matching whole words.
   * `\B`: Matches a non-word boundary.
   * , , , etc.: Match newline, tab, carriage return, etc.
5. **Anchors (Matching Positions):**
   * `^`, `$`: Start/end of string/line (as mentioned before).
   * `\b`, `\B`: Word boundaries (as mentioned before).
   * `\A`: Matches only at the absolute start of the string (ignores multiline mode).
   * `\Z`: Matches only at the absolute end of the string or before a final newline (ignores multiline mode mostly).
   * `\z`: Matches only at the absolute end of the string (ignores multiline mode).
6. **Modifiers/Flags:** These change how the entire pattern is interpreted. The syntax for applying them varies (e.g., `/pattern/i` in Perl/PHP, `(?i)pattern` inline, flags argument in functions).
   * `i`: Case-insensitive matching.
   * `m`: Multiline mode (`^` and `$` match start/end of lines within the string, not just the absolute start/end).
   * `s`: Single line / dotall mode (`.` matches newline characters).
   * `x`: Extended mode (ignore unescaped whitespace in the pattern, allow `#` comments). Useful for complex regex readability.
   * `U`: Ungreedy mode (makes `*`, `+`, `?`, `{n,m}` lazy by default; `??`, `+?` etc. become greedy).

***
