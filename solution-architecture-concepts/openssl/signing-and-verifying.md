# Signing and Verifying

{% embed url="https://pagefault.blog/2019/04/22/how-to-sign-and-verify-using-openssl/" %}

**Steps to Create a Signature**

1.  **Generate a Private Key:**

    Bash

    ```
    # Generate 4096-bit RSA private key and extract public key
    openssl genrsa -out key.pem 4096

    ```

    This creates a 2048-bit RSA private key and stores it in the file `private.key`.
2.  **Generate a Public Key:**

    Bash

    ```
    openssl rsa -in key.pem -pubout > key.pub
    ```

    This extracts the corresponding public key from your private key and stores it in `public.key`.
3.  **Create signature:**

    Bash

    ```
    openssl dgst -sign key.pem -keyform PEM -sha256 -out data.zip.sign -binary data.zip
    ```

    This generates a hash and then signs it using the private key
4.  **verify signature:**

    Bash

    ```
    openssl dgst -verify key.pub -keyform PEM -sha256 -signature data.zip.sign -binary data.zip
    Verified OK
    ```



