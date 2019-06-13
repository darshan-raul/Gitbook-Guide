# SSH

## SSH overview

```text
man ssh
```

**NAME** ssh — OpenSSH SSH client \(remote login program\)

**SYNOPSIS**

```text
 ssh [-b bind_address] [-c cipher_spec] [-D [bind_address:]port] [-E log_file]
         [-e escape_char] [-F configfile] [-I pkcs11] [-i identity_file] [-J [user@]host[:port]] [-L address]
         [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port] [-Q query_option] [-R address]
         [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]] [user@]hostname [command]
```

* The SSH protocol \(also referred to as Secure Shell\) is a method for secure remote login from one computer to another. It is a _secure alternative to the non-protected login protocols \(such as telnet, rlogin\) and insecure file transfer methods \(such as FTP\)_.
* SSH has a **client/server architecture**
* An SSH server program,typically installed and run by a system administrator, accepts or rejects incoming connections to its host computer. Users then run SSH client programs, typically on other computers, to make requests of the SSH server.

  All communications between clients and serversare securely encrypted and protected from modification.

### Remote Terminal Sessions with ssh

```text
ssh -l pat shell.isp.com
```

or

```text
ssh pat@shell.isp.com
```

On first contact, SSH establishes a secure channel between the client and the server so all transmissions between them are encrypted. The client then prompts for your password, which it supplies to the server over the secure channel. The server authenticates you by checking that the password is correct and permits the login. All subsequent client/server exchanges are protected by that secure channel

### Initial server key discovery

### Executing commands remotely

### Redirecting commands’ input and output

### SSH forwarding

