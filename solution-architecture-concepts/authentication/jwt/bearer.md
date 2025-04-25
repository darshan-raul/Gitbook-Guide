# Bearer

&#x20;**JWT (JSON Web Token)** itself is the _type_ or _format_ of the token. **"Bearer"** is not a type _of_ JWT, but rather a **token usage scheme** defined within the OAuth 2.0 authorization framework. It specifies _how_ a token (which _could_ be a JWT, but doesn't have to be) should be presented to access a protected resource.

Here's a detailed explanation:

1. **JWT (JSON Web Token) - The Token Format**
   * **What it is:** JWT is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed.<sup>1</sup>
   * **Structure:** A JWT consists of three parts separated by dots (`.`):
     *   **Header:** Contains metadata about the token, typically the token type (`typ`, usually "JWT") and the signing algorithm used (`alg`, e.g., HMAC SHA256 or RSA). This part is Base64Url encoded.JSON

         ```
         {
           "alg": "HS256",
           "typ": "JWT"
         }
         ```
     *   **Payload (Claims):** Contains the statements ("claims") about an entity (typically, the user) and additional data. Claims are pieces of information asserted about a subject. There are three types:

         * **Registered Claims:** Predefined claims recommended for interoperability (e.g., `iss` (issuer), `exp` (expiration time), `sub` (subject), `aud` (audience)).
         * **Public Claims:** Defined by those using JWTs, but should be defined in the IANA JSON Web Token Registry or be collision-resistant URIs to avoid conflicts.
         * **Private Claims:** Custom claims created to share information between parties that agree on using them; they are neither registered nor public. This part is also Base64Url encoded.

         JSON

         ```
         {
           "sub": "1234567890",
           "name": "John Doe",
           "admin": true,
           "iat": 1516239022,
           "exp": 1516242622
         }
         ```
     *   **Signature:** To verify the integrity of the token (that the header and payload haven't been tampered with) and, if using asymmetric keys, to verify the sender's identity. It's created by taking the encoded header, the encoded payload, a secret (if using HMAC) or a private key (if using RSA/ECDSA), and signing them with the algorithm specified in the header.

         ```
         HMACSHA256(
           base64UrlEncode(header) + "." +
           base64UrlEncode(payload),
           secret)
         ```
   * **Characteristics:**
     * **Compact:** Small size allows them to be sent in URLs, POST parameters, or HTTP headers.
     * **Self-contained:** The payload contains all the required information about the user, reducing the need for multiple database lookups on the server side for verification (though checking against revocation lists might still be needed).
     * **Signed:** Ensures data integrity and authenticity. Can also be encrypted (using JWE - JSON Web Encryption) if payload confidentiality is required, though signed JWTs (JWS - JSON Web Signature) are more common for authorization.
2. **Bearer Authentication Scheme - The Token Usage**
   *   **What it is:** Defined in RFC 6750 (The OAuth 2.0 Authorization Framework: Bearer Token Usage), "Bearer" is an authentication scheme used with HTTP. When a client wants to access a protected resource, it includes an `Authorization` header with the value `Bearer <token>`.HTTP

       ```
       Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM...
       ```
   * **Meaning:** The term "Bearer" signifies that **anyone** who possesses (bears) the token can use it to gain access. The server doesn't need further proof of identity beyond validating the token itself (checking its signature, expiration, issuer, audience, etc.).
   * **Security:** Because possession grants access, bearer tokens must be protected. They should _only_ be transmitted over secure channels (HTTPS) to prevent interception. If a bearer token is leaked, an attacker can use it.
   * **Relation to JWT:** While any string could potentially be a bearer token, **JWTs are very commonly used as the `<token>` part in the `Bearer` scheme**. This is because JWTs are well-suited for carrying the necessary authorization information (like user ID, roles, permissions, expiration) in a verifiable, compact format.

**Other Related Concepts (Often Used Alongside JWTs):**

While JWT is the format and Bearer is the common usage scheme, tokens often play specific roles within authentication/authorization flows like OAuth 2.0 and OpenID Connect (OIDC):

* **Access Token:**
  * **Purpose:** Grants the client access to specific protected resources on behalf of a user.
  * **Format:** Often a JWT, but can also be an opaque string (where the server looks up the details). When it's a JWT, it usually contains claims like scope (`scp`), audience (`aud`), expiration (`exp`), subject (`sub`), etc.
  * **Usage:** Typically sent using the `Bearer` scheme in the `Authorization` header.
  * **Lifespan:** Usually short-lived (minutes to hours) to limit the damage if leaked.
* **Refresh Token:**
  * **Purpose:** Used by the client to obtain a _new_ access token when the current one expires, without requiring the user to log in again.
  * **Format:** Usually an _opaque_, cryptographically random string, _not_ typically a JWT. Its value is stored securely by the authorization server and associated with the user's session.
  * **Usage:** Sent to a specific token endpoint on the authorization server. It is _not_ sent to resource servers and not usually used with the `Bearer` scheme.
  * **Lifespan:** Long-lived (days, weeks, months), but can be revoked by the server. Requires very secure storage on the client.
* **ID Token:**
  * **Purpose:** Defined by OpenID Connect (OIDC), which builds on OAuth 2.0. It provides information about the _authentication_ event and claims about the end-user (identity information) directly to the client application.
  * **Format:** **Always a JWT**. Contains claims like `iss`, `sub`, `aud`, `exp`, `iat`, `auth_time`, `nonce`, and potentially user profile information (name, email, etc.).
  * **Usage:** Sent from the authorization server to the client _after_ successful user authentication. It's primarily meant for the _client_ to understand who the user is, not usually for accessing APIs (that's the Access Token's job). It's not typically sent in an `Authorization: Bearer` header to resource servers.

**In Summary:**

* **JWT:** Is the _format_ of the token (Header.Payload.Signature).
* **Bearer:** Is the _scheme_ for presenting a token (`Authorization: Bearer <token>`). JWTs are frequently used as the `<token>` in this scheme.
* **Access Token, Refresh Token, ID Token:** Describe the _functional roles_ tokens play in authentication/authorization protocols like OAuth 2.0 and OIDC. Access Tokens and ID Tokens are often (or always, for ID Tokens) JWTs, while Refresh Tokens usually are not. Access Tokens are typically used with the Bearer scheme.
