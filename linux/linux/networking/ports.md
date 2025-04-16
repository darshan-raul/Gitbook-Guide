# ports

In Linux and other Unix-like systems, network ports are categorized into three main ranges based on their intended use and the privileges required to bind to them.

***

### 🔒 1. Well-Known Ports (0–1023)

* **Range**: 0 to 1023
* **Description**: These ports are reserved for core services and protocols. Binding to these ports typically requires superuser (root) privileges.
* **Examples**:
  * Port 22: SSH (Secure Shell)
  * Port 80: HTTP
  * Port 443: HTTPS
  * Port 25: SMTP (Simple Mail Transfer Protocol)
  * Port 53: DNS (Domain Name System)
* **Usage**: Essential for fundamental network services and protocols.

***

### 📝 2. Registered Ports (1024–49151)

* **Range**: 1024 to 49151
* **Description**: These ports are assigned by the Internet Assigned Numbers Authority (IANA) for specific services upon request. They are used by user processes or applications that are not part of the core operating system.
* **Examples**:
  * Port 3306: MySQL Database
  * Port 5432: PostgreSQL Database
  * Port 8080: Alternative HTTP port (commonly used for web servers)
* **Usage**: Commonly utilized by third-party applications and services.

***

### 🔄 3. Dynamic/Private/Ephemeral Ports (49152–65535)

* **Range**: 49152 to 65535
* **Description**: These ports are not assigned to any specific service by IANA and are used for temporary or private purposes. Operating systems typically allocate these ports dynamically when establishing outbound connections.
* **Usage**: Used for client-side communications, such as when a client connects to a server, the client's operating system assigns an ephemeral port for the duration of the connection.

***

#### Additional Notes

* **Port 0**: Reserved and typically not used for network communications. In programming, specifying port 0 allows the operating system to assign an available ephemeral port automatically.
* **Privileged Ports**: On Unix-like systems, ports below 1024 are considered privileged. Binding to these ports requires elevated permissions, ensuring that only trusted system services can use them.

***

For a comprehensive list of port assignments and their associated services, you can refer to the [IANA Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers).
