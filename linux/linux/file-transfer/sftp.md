# SFTP

#### Overview of SFTP

**SFTP (SSH File Transfer Protocol)** is a secure method for transferring files between remote systems. Unlike FTP, SFTP encrypts both commands and data, preventing passwords and sensitive information from being transmitted in clear text over the network.

**Key Features:**

* **Security:** Utilizes SSH for encryption, ensuring secure file transfers.
* **Authentication:** Supports various methods including password-based and key-based authentication.
* **Integration:** Can be easily integrated into existing SSH infrastructure.

#### Setting Up SFTP on a Linux Server

**Step 1: Install OpenSSH Server**

Most Linux distributions come with OpenSSH server pre-installed. If not, you can install it using the package manager:

For **Ubuntu/Debian**:

```sh
sudo apt-get update
sudo apt-get install openssh-server
```

For **CentOS/RHEL**:

```sh
sudo yum install openssh-server
```

**Step 2: Configure SSH for SFTP**

Edit the SSH daemon configuration file:

```sh
sudo nano /etc/ssh/sshd_config
```

Add or modify the following lines to configure the SFTP subsystem:

```sh
Subsystem sftp internal-sftp

Match Group sftpusers
    ChrootDirectory /home/%u
    ForceCommand internal-sftp
    AllowTcpForwarding no
    X11Forwarding no
```

* **Subsystem sftp internal-sftp**: This line specifies that the internal SFTP server should be used.
* **Match Group sftpusers**: This block applies the settings to users in the `sftpusers` group.
* **ChrootDirectory /home/%u**: Restricts the user to their home directory.
* **ForceCommand internal-sftp**: Forces the use of SFTP instead of SSH.
* **AllowTcpForwarding no** and **X11Forwarding no**: Disables forwarding capabilities for security.

**Step 3: Create SFTP Group and Users**

Create a group for SFTP users:

```sh
sudo groupadd sftpusers
```

Create a user and assign them to the `sftpusers` group:

```sh
sudo useradd -m -G sftpusers -s /sbin/nologin sftpuser1
```

Set a password for the new user:

```sh
sudo passwd sftpuser1
```

**Step 4: Set Directory Permissions**

Set the appropriate permissions for the user's home directory:

```sh
sudo chown root:root /home/sftpuser1
sudo chmod 755 /home/sftpuser1
```

Create a directory for file uploads:

```sh
sudo mkdir /home/sftpuser1/uploads
sudo chown sftpuser1:sftpuser1 /home/sftpuser1/uploads
```

**Step 5: Restart SSH Service**

Restart the SSH service to apply the changes:

```sh
sudo systemctl restart ssh
```

**Step 6: Connecting to the SFTP Server**

From a client machine, you can connect to the SFTP server using the following command:

```sh
sftp sftpuser1@server_ip
```

Replace `sftpuser1` with the actual username and `server_ip` with the server's IP address.

#### Additional Considerations

* **Firewall:** Ensure that the firewall allows SSH connections (port 22 by default).
* **Key-Based Authentication:** For enhanced security, consider setting up key-based authentication.
* **Logging and Monitoring:** Regularly monitor logs for any unauthorized access attempts or issues.

#### ---

#### AllowTcpForwarding no

**TCP Forwarding** allows SSH users to forward TCP connections from the client to the server or vice versa. It can be useful for tunneling traffic through a secure SSH connection but can also pose security risks if misused.

**AllowTcpForwarding no** disables TCP forwarding, preventing users from using SSH to forward ports, which enhances security by limiting the potential attack vectors. This directive is particularly important in scenarios where users are only intended to transfer files using SFTP and should not have the ability to route network traffic through the SSH server.

#### X11Forwarding no

**X11 Forwarding** allows remote graphical applications to be displayed on the local machine. It is a feature of SSH that enables a secure tunnel for X11 traffic, permitting the execution of graphical applications over the SSH connection.

**X11Forwarding no** disables X11 forwarding, preventing users from running graphical applications remotely and displaying them locally. This directive is useful for enhancing security, especially when users are only intended to use the server for file transfers via SFTP, and should not have the capability to run and display graphical applications over the network.

#### Summary

* **AllowTcpForwarding no**: Disables the ability to forward TCP connections, preventing users from using the SSH server as a tunnel for other network services, thereby reducing potential security risks.
* **X11Forwarding no**: Disables the forwarding of X11 (graphical application) connections, ensuring that users cannot run remote graphical applications through the SSH connection, further enhancing security.

Both of these settings are aimed at restricting the capabilities of users to only what is necessary (in this case, SFTP file transfers), thereby adhering to the principle of least privilege and reducing the attack surface of the server.
