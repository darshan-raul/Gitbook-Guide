# TCP

{% embed url="https://www.youtube.com/watch?v=JWUsO1vmPcE" %}

**TCP (Transmission Control Protocol)**

* **Reliability:** TCP is a connection-oriented protocol, meaning it establishes a virtual circuit before sending any data. It guarantees delivery and maintains the order of data packets.
* **Flow Control:** TCP prevents the sender from overwhelming the receiver by implementing a dynamic window size. If the receiver is slower, it communicates to the sender to slow down the transmission rate.
* **Congestion Control:** TCP uses mechanisms like slow start and congestion avoidance algorithms to regulate traffic on the network, preventing collapse under heavy loads.
* **Three-way Handshake (SYN, SYN-ACK, ACK):** This is how TCP initiates reliable connections and synchronizes sequence numbers.
* **FIN-ACK Sequence:** TCP uses a graceful four-step process to terminate connections, ensuring all data is transferred and acknowledged.
