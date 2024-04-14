# Keystore

Here's a guide on using a Java KeyStore (JKS) to store a certificate chain and a server's private key.

**Prerequisites:**

* **Java Development Kit (JDK):** Ensure you have a JDK installed. You'll need the `keytool` utility.
* **Existing Certificates and Key:** You should have the following:
  * **Server's Private Key:** Usually in a PEM format file (e.g., `server.key`)
  * **Server's Certificate:** PEM format (e.g., `server.crt`)
  * **Certificate Chain:** PEM format, including any intermediate certificates, and optionally the root CA certificate.

**Steps**

1. **Convert to PKCS#12 (if necessary):**
   *   If your private key and certificates aren't already PKCS#12, convert them:Bash

       ```
       openssl pkcs12 -export -out server.p12 -inkey server.key -in server.crt -certfile chain.crt 
       ```

       (Replace the filenames with your actual files)
2.  **Create a New JKS Keystore:**

    Bash

    ```
    keytool -genkeypair -alias serveralias -keyalg RSA -keysize 2048 -keystore storename.jks -dname "CN=yourdomain.com" 
    ```

    * Replace `serveralias` with a meaningful name.
    * Replace `yourdomain.com` with the domain name associated with your certificate.
    * You'll be prompted for passwords and some basic information.
3.  **Import the PKCS#12 bundle:**

    Bash

    ```
    keytool -importkeystore -deststorepass storepassword -destkeystore storename.jks -srckeystore server.p12 -srcstoretype PKCS12 -alias serveralias
    ```

    * Replace `storepassword` with the password set in step 2.

**Explanation**

* **Step 1 (Conversion):** OpenSSL is used to bundle the private key, server certificate, and certificate chain into a PKCS#12 file, as this is easier for the Java `keytool` to import.
* **Step 2 (Keystore Creation):** The `keytool` creates a new keystore file (`storename.jks`) and generates a public/private key pair under the specified 'alias'.
* **Step 3 (Import):** The PKCS#12 bundle (containing everything you need) is imported into the JKS keystore, associated with the 'alias' you set.

**Important Notes:**

* **Passwords:** The `keytool` will prompt you for a keystore password and also a separate password for the key entry. Choose strong passwords.
* **Certificate Chain Order:** If you imported multiple certificates, ensure the correct order within the chain (certificate chain file, PKCS#12 bundle).
* **Security:** Protect your JKS keystore file and the passwords associated with it.

