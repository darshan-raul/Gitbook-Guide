# Sockets

Sure, let's break this down:

#### Sockets as Linux File Descriptors

* **File Descriptors**: In Unix-like operating systems, a file descriptor is a unique identifier for a file or a resource like a socket. File descriptors are used by processes to perform input/output operations.
* **Sockets as File Descriptors**: A socket is one type of file descriptor that specifically facilitates network communication. Just like file descriptors can point to files, sockets point to network connections.

#### Communication End-Points

* **Communication End-Points**: A socket serves as an end-point for sending and receiving data across a network. It acts as an interface between the application layer (where your programs run) and the transport layer (which handles the actual transmission of data over the network).

#### IP Address and Port Number

* **IP Address**: This is the address of the device on the network. It ensures that the data is sent to the correct device.
* **Port Number**: This is a numerical identifier within the device that helps direct the data to the correct application or service. Different services (like web servers, email servers, etc.) listen on different ports.

#### Putting It Together

* When a process wants to communicate over a network, it creates a socket.
* This socket is assigned a file descriptor by the operating system.
* The socket binds to a combination of an IP address and a port number.
* The IP address directs the data to the correct device, and the port number directs the data to the correct application or service on that device.

#### Example

Imagine you have a web server running on your device. Here's what happens in terms of sockets:

1. **Creating a Socket**: The web server process creates a socket.
2. **Binding**: The socket binds to the device's IP address (e.g., `192.168.1.10`) and a port number (e.g., `80` for HTTP).
3. **Listening**: The web server listens for incoming connections on this socket.
4. **Communication**: When a client (like a web browser) wants to connect to the web server, it uses the server's IP address and port number to establish a connection. The server's socket receives this connection, and communication can begin.

In summary, a socket in Linux is a file descriptor that represents one end of a network communication link, identified by a device's IP address and a specific port number.



***

Sure, let's go through the steps to view sockets on your machine and create and use a socket. This guide assumes you're using a Linux-based system.

#### Viewing Existing Sockets

1.  **List All Sockets**

    ```bash
    ss -a
    ```

    This command lists all the sockets currently in use on your machine, including TCP, UDP, and Unix sockets.
2.  **List TCP Sockets**

    ```bash
    ss -t
    ```

    This command lists all the TCP sockets.
3.  **List UDP Sockets**

    ```bash
    ss -u
    ```

    This command lists all the UDP sockets.
4.  **List Listening Sockets**

    ```bash
    ss -l
    ```

    This command lists all the sockets that are currently in a listening state.

#### Creating and Using a Socket

Let's create a simple client-server application using Python to demonstrate socket usage.

**Server Side**

1.  **Create a Server Script**

    Create a file named `server.py` with the following content:

    ```python
    import socket

    # Create a socket object
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    # Get local machine name
    host = socket.gethostname()
    port = 12345

    # Bind to the port
    server_socket.bind((host, port))

    # Queue up to 5 requests
    server_socket.listen(5)

    print(f"Server listening on {host}:{port}")

    while True:
        # Establish a connection
        client_socket, addr = server_socket.accept()
        print(f"Got a connection from {addr}")

        # Send a thank you message to the client
        message = 'Thank you for connecting'
        client_socket.send(message.encode('ascii'))

        # Close the connection
        client_socket.close()
    ```
2.  **Run the Server**

    ```bash
    python3 server.py
    ```

    This will start the server and listen for incoming connections on the specified port.

**Client Side**

1.  **Create a Client Script**

    Create a file named `client.py` with the following content:

    ```python
    import socket

    # Create a socket object
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    # Get local machine name
    host = socket.gethostname()
    port = 12345

    # Connection to hostname on the port
    client_socket.connect((host, port))

    # Receive no more than 1024 bytes
    message = client_socket.recv(1024)
    print(message.decode('ascii'))

    # Close the connection
    client_socket.close()
    ```
2.  **Run the Client**

    ```bash
    python3 client.py
    ```

    This will connect to the server, receive a message, and print it out.

#### Explanation

* **Server Script (`server.py`)**:
  * Creates a TCP socket.
  * Binds it to the local machine's hostname and port `12345`.
  * Listens for incoming connections.
  * Accepts a connection and sends a message to the client.
* **Client Script (`client.py`)**:
  * Creates a TCP socket.
  * Connects to the server using the local machine's hostname and port `12345`.
  * Receives a message from the server and prints it.

By following these steps, you can view existing sockets on your machine and create a simple socket-based client-server application using Python.
