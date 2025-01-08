# PKI

Here's a comprehensive tutorial using OpenSSL to create a Root CA, generate a certificate chain, sign a request, and verify the signature from the client side:

**Prerequisites:**

* OpenSSL installed on your system.

**Steps:**

**1. Create the Root CA**

1.  **Generate private key:**

    Bash

    ```
    openssl genrsa -out root-ca.key 4096
    ```
2.  **Create self-signed certificate:**

    Bash

    ```
    openssl req -x509 -new -nodes -key root-ca.key -sha256 -days 3650 -out root-ca.crt 
    ```

    * You'll be prompted to enter information (Country, State, Organization, etc.). Ensure this information aligns with your intended use of the CA.

**2. Create a Server Certificate Chain**

1.  **Generate private key for the server:**

    Bash

    ```
    openssl genrsa -out server.key 2048 
    ```
2.  **Create a Certificate Signing Request (CSR):**

    Bash

    ```
    openssl req -new -key server.key -out server.csr
    ```

    * Enter information, importantly the Common Name (domain name of the server, e.g., [www.example.com](https://www.example.com/)).
3.  **Sign the CSR with Root CA:**

    Bash

    ```
    openssl x509 -req -in server.csr -CA root-ca.crt -CAkey root-ca.key -CAcreateserial -out server.crt -days 365 -sha256 
    ```



**3. Client-Side Verification**

1. **Copy the Root CA certificate to client machines:** `root-ca.crt`
2. **Client Code:** (Language-specific, here's the concept)
   * Load the Root CA certificate as a trusted certificate.
   * When connecting to the server, validate the server's certificate chain against the trusted Root CA.
   * Libraries like OpenSSL or your programming language's TLS library usually provide functions for this.

**Example: Verification using OpenSSL command line**

Bash

```
openssl verify -CAfile root-ca.crt server.crt 
```

Output of `server.crt: OK` indicates successful verification.

**Important Notes:**

* **Secure Storage:** Protect the Root CA's private key (`root-ca.key`) with the utmost security, preferably offline or in an HSM.
* **Distribution:** Distribute the Root CA certificate (`root-ca.crt`) to clients or systems that need to trust certificates issued by your CA.
* **Revocation:** Have a mechanism for revoking certificates if needed.
* **Production-Readiness:** This outlines the basic process; production systems may involve intermediate CAs and more complex certificate management.

***

### What is this signing stuff actually?

Here's a deeper dive into what happens during the signing of a Certificate Signing Request (CSR) along with the associated concepts:

**1. Core Components**

* **CSR:** The starting point - contains the public key of the entity requesting the certificate, their identity information (domain name, organization, etc.), and a digital signature for verification.
* **Certificate Authority (CA):** The entity responsible for signing the CSR and issuing the certificate. The CA has its own private key.
* **Digital Signature:** A cryptographic mechanism ensuring authenticity and integrity.

**2. The Signing Process in Detail**

1. **Hashing the CSR:** A cryptographic hash function (e.g., SHA-256) is applied to the entire contents of the CSR, producing a unique, fixed-size digest (hash value).
2. **CA's Private Key Encryption:** The CA encrypts this hash value using its private key. This encrypted hash becomes the digital signature that will be appended to the certificate.
3. **Building the Certificate:** The CA creates an X.509 certificate structure containing:
   * The public key and identity information from the CSR.
   * Issuer details (The CA's information).
   * Validity periods (start and end dates).
   * Extensions (optional, defining permitted usage, etc.).
   * The CA's digital signature (from step 2).

**3. Concepts Involved**

* **Public Key Cryptography (PKI):** The entire signing process relies on asymmetric cryptography. The CA's private key is used for signing, and the corresponding public key (available in the CA's own certificate) is used for verification.
* **Cryptographic Hash Functions:** Ensures integrity. Any change to the CSR would produce a different hash, rendering the digital signature invalid.
* **X.509 Certificate Standard:** Defines the structure and data fields contained in a digital certificate.
* **Certificate Authorities:** Trusted entities that issue and vouch for the authenticity of certificates within a PKI system.
* **Trust Chain:** Built on a hierarchy of CAs. Your browser trusts a set of Root CAs, and those Root CAs sign certificates of intermediate CAs, ultimately leading to the signing of your server's certificate, forming a chain.
* **Certificate Signing Request (CSR):** The formal request that bundles the requester's public key and identity information, ready for signing.

**4. Why Signing Matters**

* **Identity Verification:** The CA, to some degree, verifies the information supplied in the CSR before issuing the certificate.
* **Authenticity:** The digital signature assures that the certificate was truly issued by the CA and not modified in transit.
* **Trust Establishment:** Browsers and clients trust certificates signed by recognized CAs, establishing the foundation for secure HTTPS connections.

<mark style="background-color:purple;">**Here's a breakdown of how the digital signature is verified during an OpenSSL**</mark><mark style="background-color:purple;">**&#x20;**</mark><mark style="background-color:purple;">**`verify`**</mark><mark style="background-color:purple;">**&#x20;**</mark><mark style="background-color:purple;">**process or a similar certificate verification within a client application**</mark>:

**Prerequisites:**

* **CA's Public Key:** The client needs the trusted root CA's public key (contained in its certificate).
* **Signed Certificate:** The certificate that needs to be verified (e.g., your server's certificate).

**Steps of Digital Signature Verification**

1. **Retrieving the Digital Signature:** The digital signature is extracted from the signed certificate. Remember, this signature was created by the CA encrypting a hash of the certificate's contents with the CA's private key.
2. **Hash Calculation:** The same cryptographic hash function used during signing is applied to the current certificate's contents. This generates a new hash value.
3. **Decryption of the Digital Signature:** The CA's public key (from their certificate) is used to decrypt the extracted digital signature. This decryption process recovers the original hash value that was encrypted by the CA's private key.
4. **Hash Comparison:** The newly calculated hash value (step 2) is compared with the decrypted hash value (step 3). **If they match, this proves:**
   * **Integrity:** The certificate hasn't been tampered with since the CA signed it.
   * **Authenticity:** The certificate was indeed issued by the specific CA (since only they possess the private key that could create a signature validated by their public key).

**Conceptual Explanation**

Essentially, the verification process works backwards from the signing process:

* **Signing:**
  1. Hash the certificate data.
  2. Encrypt the hash with the CA's private key.
* **Verification:**
  1. Decrypt the signature with the CA's public key (revealing the original hash).
  2. Calculate a fresh hash of the certificate data.
  3. Compare the hashes – a match indicates a valid signature.

