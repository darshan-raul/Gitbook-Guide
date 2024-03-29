# PAM

**Module 1: PAM Foundations**

* **Understanding PAM:**
  * What is PAM (Pluggable Authentication Modules)?
  * The modular architecture of PAM and its flexibility.
  * Core PAM configuration files (/etc/pam.d/\*)
  * The four PAM management groups: auth, account, password, session.
* **Essential PAM Modules:**
  * `pam_unix.so` - Traditional Unix password authentication.
  * `pam_cracklib.so` - Enforcing password complexity rules.
  * `pam_limits.so` - Setting resource limits for user sessions (e.g., process limits, open files).
  * `pam_tally2.so` - Account lockout after failed logins.

**Module 2: Hardening Scenarios with PAM**

* **Enforcing Strong Passwords:**
  * Using `pam_cracklib.so` to mandate length, complexity, and password history.
  * Exploring `pam_passwdqc.so` for even more advanced password controls.
* **Account Lockout and Login Restrictions:**
  * Configuring `pam_tally2.so` for temporary lockouts after failed attempts.
  * Using `pam_time.so` for time-based access restrictions (logins allowed only during certain hours).
  * Using `pam_securetty.so` to define allowed login terminals.
* **Resource Limitation:**
  * Applying `pam_limits.so` to control system resource usage by users and to prevent accidental resource exhaustion.
* **Enhanced Security with Additional Modules:**
  * `pam_wheel.so` - Restricting 'su' access to members of the 'wheel' group.
  * `pam_faillock.so` - A more detailed alternative to `pam_tally2.so`.
  * `pam_access.so` - Fine-grained access control based on user, origin and time.

**Module 3: Practical Examples and Best Practices**

* **Scenario-Based Configurations:**
  * Hardening a standard user workstation.
  * Securing a critical server.
  * Implementing two-factor authentication with PAM (`pam_google_authenticator.so`).
* **Best Practices for PAM Configuration:**
  * Thorough testing of changes before deploying to production.
  * Principle of least privilege.
  * Logging and auditing with PAM

**Module 4: Deeper Dive into PAM (Optional)**

* **Advanced Configuration Concepts:**
  * PAM stacks: understanding the control flags (required, requisite, sufficient, optional)
  * Writing your own custom PAM modules (if you're up for a challenge).

**How We'll Learn**

* **Theory + Practice:** Each module will have theoretical explanations followed by hands-on configuration examples with sample `/etc/pam.d` files.
* **Scenario-Based Learning:** We'll work through common hardening use cases for contextual understanding.

***

## Module 1



Absolutely! Let's dive into Module 1, where we'll lay the groundwork for understanding PAM.

**Module 1: PAM Foundations**

**1. What is PAM?**

* **Pluggable Authentication Modules (PAM)** is a powerful and flexible framework within Linux systems that handles various aspects of authentication.
* Think of PAM as a collection of building blocks (modules) that system administrators can arrange in different ways to manage how users log in, change their passwords, and interact with different services on the system.

**2. PAM's Modular Architecture**

* **The Core:** PAM libraries are responsible for loading modules and interpreting configuration files located within `/etc/pam.d/`.
* **PAM Modules:** These are the individual building blocks (.so files) that implement specific authentication tasks. Each module is designed for a purpose:
  * Password checking (`pam_unix.so`)
  * Enforcing password complexity (`pam_cracklib.so`)
  * Implementing account lockouts (`pam_tally2.so`)
  * ...and many more!

**3. Key PAM Configuration Files**

* PAM configuration lives primarily within the `/etc/pam.d/` directory.
* Each service (like sshd, login, sudo) typically has a corresponding configuration file in this directory.
* Example: `/etc/pam.d/sshd` would control authentication rules for the SSH service.

**4. PAM Management Groups**

PAM modules are organized into four primary management groups:

* **auth:** Handles the core task of user authentication (verifying passwords, checking 2FA tokens, etc.)
* **account:** Focuses on account restrictions (is the account expired, is login allowed at this time, etc.)
* **password:** Responsible for managing password changes and password quality enforcement.
* **session:** Handles tasks related to setting up and tearing down a user's session (mounting home directory, setting environment variables, etc.)

**Let's Look at a Sample Configuration**

Here's a simplified example of the `/etc/pam.d/login` file:

```
auth    required    pam_unix.so
account required    pam_unix.so
password required   pam_cracklib.so retry=3
session required    pam_unix.so
```

* Each line represents a PAM stack. PAM will work through this file line by line when someone tries to log in.
* Modules are followed by control flags (`required`, `sufficient`, etc.) that dictate the flow of the authentication process.



Fantastic! Let's move on to exploring some of the most important PAM modules you'll regularly encounter in Linux hardening scenarios.

**Essential PAM Modules**

1. **pam\_unix.so:**
   * This is the cornerstone module. It handles traditional Linux password authentication, checking against the `/etc/passwd` and `/etc/shadow` files.
   * You'll see this used for authentication tasks in most PAM configurations.
2. **pam\_cracklib.so:**
   * This module is all about enforcing password quality. With it, you can set rules like:
     * Minimum password length
     * Requiring a mix of lowercase, uppercase, numbers, and special characters.
     * Preventing use of dictionary words.
     * Checking against a history of previous passwords.
3. **pam\_limits.so**
   * This module lets you control resource limits for users to prevent denial of service situations. Examples of limits:
     * Maximum number of processes
     * Maximum open files
     * Maximum CPU time
4. **pam\_tally2.so (and pam\_faillock.so):**
   * Implements account lockout policies.
   * `pam_tally2.so` typically provides a simple counter for failed login attempts. After a set threshold, the account is locked for a set time.
   * `pam_faillock.so` offers more detailed control, like distinguishing between different failure types and allowing for manual unlocking by an administrator.

**Example Scenario**

Let's imagine hardening local logins. Our `/etc/pam.d/login` file might look like this:

```
auth    requisite   pam_tally2.so deny=5 unlock_time=300
auth    required    pam_unix.so 
auth    sufficient  pam_cracklib.so retry=3 minlen=10 difok=4
account required    pam_unix.so 
```

**Explanation**

* `pam_tally2.so`: Locks the account after 5 failed attempts for 5 minutes.
* `pam_unix.so`: Checks the user's password.
* `pam_cracklib.so`: Requires a password at least 10 characters long, different from the last 4, and the user is given 3 attempts to get it right.

**Key Points:**

* **Don't Just Copy:** Configuration examples are great, but understanding why you're using specific modules and how they work together is essential for proper hardening.
* **PAM is Powerful:** Many more modules exist for specialized tasks like LDAP authentication, smart card logins, etc.



Excellent! Let's do a quick rundown of configuring some of the core PAM modules we've discussed, and then dive into those hardening scenarios.

**Configuring Core PAM Modules**

Here's a breakdown of how to use the essential modules in your `/etc/pam.d/` configuration files.

**1. pam\_unix.so**

* **Syntax:** `auth required pam_unix.so [options]`
* **Common Options:**
  * `nullok`: Allows logins with empty passwords (generally not recommended).
  * `try_first_pass`: Attempts to use the previous password from a different authentication stack (useful for password chaining).
  * `shadow`: Ensures the use of shadow passwords.

**2. pam\_cracklib.so**

* **Syntax:** `password sufficient pam_cracklib.so [options]`
* **Common Options:**
  * `retry=N`: Number of retries allowed for a password change.
  * `minlen=N`: Minimum password length.
  * `difok=N`: Number of characters that must differ from the previous password.
  * `lcredit=N`: Minimum number of lowercase characters required.
  * `ucredit=N`: Minimum number of uppercase characters required.
  * `dcredit=N`: Minimum number of digits required
  * `ocredit=N`: Minimum number of special characters required.

**3. pam\_limits.so**

* **Configuration:** Traditionally, limits would be configured in `/etc/security/limits.conf`. This module reads those settings.
*   **Example Entries in limits.conf:**

    ```
    *               hard    nofile          4096
    someuser        soft    nproc           20
    @students       hard    maxlogins       3
    ```

    * Limits can be 'hard' (strictly enforced) or 'soft' (a user can temporarily exceed with 'ulimit').
    * They target individual users, groups (prefixed with @), or '\*' for a wildcard match.

**4. pam\_tally2.so**

* **Syntax:** `auth requisite pam_tally2.so [options]`
* **Common Options:**
  * `deny=N`: Number of failed attempts before lockout.
  * `unlock_time=N`: Lockout time in seconds.
  * `file=/path/to/tally/file`: Specify where to store login attempt counts.

**Important Note:** Numerous other options exist for fine-tuning each of these modules. Always refer to the module's documentation (often found in the 'man pages') for the ultimate reference.

Absolutely! Let's put our knowledge of PAM into practice with some common hardening scenarios.

**Scenario 1: Hardening SSH Access**

**Goal:** Enhance SSH security by controlling logins and enforcing stronger passwords.

**Modifications to /etc/pam.d/sshd**

1.  **Account Lockout:**

    ```
    auth requisite pam_tally2.so deny=5 unlock_time=900
    ```

    This will lock an account for 15 minutes after 5 failed login attempts.
2.  **Password Complexity:**

    ```
    password sufficient pam_cracklib.so retry=3 minlen=12 lcredit=-1 ucredit=-1 dcredit=-1 ocredit=-1
    ```

    This requires passwords at least 12 characters long, containing at least one lowercase, uppercase, digit, and special character.
3. **Optional: Two-Factor Authentication:** If you want to go the extra step, you can explore integrating two-factor authentication with a module like `pam_google_authenticator.so`.

**Scenario 2: Restricting Root Logins**

**Goal:** Prevent direct SSH logins using the 'root' account for added security.

**Modifications to /etc/pam.d/sshd**

```
account required pam_succeed_if.so user != root quiet_success
```

This line added at the _beginning_ of the `account` section will immediately allow non-root logins while quietly denying root logins.

**Scenario 3: Time-Based Access Restrictions**

**Goal:** Allow logins from a specific user group only during working hours.

**Modifications to /etc/pam.d/sshd**

```
account requisite pam_time.so
```

Then, configure the exact allowed times within the `/etc/security/time.conf` file.

**Important Considerations**

* **Thorough Testing:** Always test PAM changes carefully in a non-production environment before rolling them out. A misconfiguration can lock you out of your system!
* **Documentation:** Maintain clear documentation about your PAM modifications. This will aid in future management and troubleshooting.

