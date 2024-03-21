# Packet flow



**1. TCP SYN-ACK Handshake:**

* **Client:** Sends a TCP SYN packet to the server, indicating it wants to initiate a connection.
* **Server:** Responds with a TCP SYN-ACK packet, acknowledging the client's SYN and sending its own SYN for data transfer.
* **Client:** Acknowledges the server's SYN-ACK with a TCP ACK packet. (3-way handshake complete)

**2. TLS Handshake:**

* **Client:** Sends a Client Hello message, specifying supported TLS versions, ciphers, and session resumption data (if applicable).
* **Server:** Responds with a Server Hello message, choosing a cipher suite and potentially requesting client certificate authentication.
* **(Optional) Client Certificate Exchange:** If requested, the client sends its certificate and potentially a certificate chain.
* **Server Key Exchange:** The server sends its public key certificate.
* **Server Hello Done:** The server signals the end of its messages.
* **Client Certificate Verify (Optional):** The client may send a message verifying the server's certificate.
* **Premaster Secret:** The client generates a random secret and encrypts it with the server's public key, sending it in the encrypted Premaster Secret message.
* **Change Cipher Spec (Client & Server):** Both parties indicate they're switching to the negotiated cipher suite.
* **Finished (Client & Server):** Both parties send a Finished message containing a hash of the handshake messages to verify data integrity. (Handshake complete, secure connection established)

**3. Application Data Flow:**

* **Client:** Sends an HTTP request message containing headers (method, URL, etc.) and potentially an encrypted payload.
* **Server:** Responds with an HTTP response message containing headers (status code, etc.) and potentially an encrypted payload.
* This data exchange can occur multiple times during the API call.

**4. FIN-ACK Termination:**

* **Client:** Sends a TCP FIN packet, indicating it wants to close the connection.
* **Server:** Acknowledges the FIN with a TCP ACK packet.
* **Server:** Sends its own TCP FIN packet to close its end.
* **Client:** Acknowledges the server's FIN with a TCP ACK packet. (Connection closed)

###
