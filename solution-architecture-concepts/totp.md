# TOTP

How Totp, the one used in google authenticator and similar such apps works behind the scenes!



**TOTP Algorithm Overview**

1. **Shared Secret:** The core is a shared secret key (usually base32-encoded) established between the user and the authenticating server.
2. **Current Time:** The current time is rounded down to a fixed interval (often 30 seconds).
3. **Counter:** This time interval is converted to a counter by dividing it by the interval length (e.g., current Unix timestamp / 30).
4. **HMAC:** An HMAC (Hash-based Message Authentication Code) function is calculated using these ingredients:
   * **HMAC Algorithm:** Usually HMAC-SHA1 (sometimes HMAC-SHA256, etc.)
   * **Secret Key:** The shared secret
   * **Counter Value:** Derived from the current time
5. **Truncation:** A portion of this HMAC output is extracted:
   * A specific offset is determined.
   * A subset of bytes are selected (usually 4).
   * This is converted into a numeric value.
6. **6-Digit Code:** <mark style="background-color:purple;">**This numeric value is then modulo'd by 1000000, yielding your 6-digit TOTP code**</mark>.

**Replication**

**Caveats:**

* **Sensitive Information:** I won't provide commands using a real secret key, as that would be a security risk. We'll simulate the process.
* **Libraries:** In real applications, use a dedicated TOTP library for your programming language. They handle the intricacies.

**Steps (Conceptual)**

1. **"Secret Key":** Generate a random string (`ABC123` in our example).
2. **Current Time:** Get the current Unix timestamp (e.g., `1677077940`).
3. **HMAC Calculation:** You'd need an HMAC-SHA1 implementation. This is where libraries are ideal. Let's assume it gives us `0xdeadbeef12345678` as output.
4. **Truncation:**
   * **Offset:** Often determined dynamically from the HMAC output. For simplicity, let's use offset 12.
   * **Extract:** Take 4 bytes starting from offset 12: `0x12345678`.
   * **Numeric Value:** Interpret this as a decimal number (305419896).
5. **Modulo:** `305419896 % 1000000 = 89896`

**Outcome:** Our generated TOTP code is `89896`.

**Important Notes**

* **Time Synchronization:** TOTP depends on the client and server having closely synchronized clocks.
