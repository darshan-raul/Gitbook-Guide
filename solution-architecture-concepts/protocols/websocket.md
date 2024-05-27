# Websocket

WebSockets provide a two-way communication channel between a web client (browser) and a web server. Unlike traditional HTTP requests, which are short-lived interactions, WebSockets establish a persistent connection, enabling real-time data exchange.

#### The Basics:

1. **Handshake:** The handshake starts with the client sending an HTTP Upgrade request to the server, specifying the WebSocket protocol.
2. **Open Connection:** If successful, the server responds with an upgrade header, establishing the WebSocket connection.
3. **Data Exchange:** Both client and server can then send and receive messages in a full-duplex manner (simultaneously). Messages are typically sent in a format like JSON for ease of use.
4. **Closing:** The connection can be closed by either party sending a close frame.

#### Advantages of WebSockets:

* **Real-time Updates:** Ideal for applications where data needs to be constantly updated on the client-side (chat applications, live dashboards, stock tickers).
* **Reduced Server Load:** Compared to constant HTTP requests, WebSockets minimize server load by utilizing a single persistent connection.
* **Bidirectional Communication:** Both server and client can initiate data flow, enhancing interactivity.

#### Advanced Concepts:

* **Message Framing:** Data is sent and received in frames. Each frame has a header that defines the type of data (text, binary) and control flags for managing the connection.
* **Subprotocols:** Optional agreements between client and server to define specific data formats or functionalities.
* **WebSocket Libraries:** Most programming languages offer libraries that simplify WebSocket development, handling the complexities of the protocol.



{% embed url="https://youtu.be/d3RJ9o7pXG4?si=ybs__UG3iqR7Ss_8" %}

{% embed url="https://youtu.be/vXJsJ52vwAA?si=CD8WPkFXILylE2L0" %}
