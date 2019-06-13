# SSH

## SSH overview

`man ssh`

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

`ssh -i private_key user@ssh_server_ip` or `ssh -l pat shell.isp.com` / `ssh pat@shell.isp.com`

On first contact, SSH establishes a secure channel between the client and the server so all transmissions between them are encrypted. The client then prompts for your password, which it supplies to the server over the secure channel. The server authenticates you by checking that the password is correct and permits the login. All subsequent client/server exchanges are protected by that secure channel

#### Using Public key authentication

SSH includes an ability to authenticate users using public keys. Instead of authenticating the user with a password, the server will verify a challenge signed by the user’s private key against its copy of the user’s public key.

Setting up public key authentication requires you to generate a public/private key pair and install the public portion on the server. It is also possible to restrict what a given key is able to do and what addresses they are allowed to log in from.

There is one client machine and one ssh server. ![](https://thor1345r75.s3.ap-south-1.amazonaws.com/sshoverview/ssh1.png)

**Generating  a public/private keypair on the client:**

1. `ssh-keygen -t rsa` you mention that the you want to use rsa algorithm to create the keypair \(you can use dsa too but rsa is by default\)
2. Provide a name to the keypair\(Else the default id\_rsa will be created in .ssh folder of user's home folder\)
3. For added security you can provide a passphrase \(It should be minimum 5 character's in length\)
4. Finally if you are able to see this randomart image that means the keypair was created successfully.

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/sshoverview/ssh2.png)

Now if you run `ll` the private and public key should be visible in the folder you ran the command in.

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/sshoverview/ssh3.png)

You can also choose to move the keys in a seperate folder if you would like to.

**Copying the public key on the SSH server:**

1. Move to the .ssh folder in the home folder of the user on ssh server
2. Edit the authorized\_keys file and add the content of the public key created on client machine here.
3. Use `cat authorized_keys` to confirm the contents of the file.

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/sshoverview/ssh5.png)

**Connect from client to SSH server:**

1. Use the command `ssh -i <private key name> <user>@<ssh server ip>` 
2. The first time your client connects to a ssh server, it asks you to verify the server’s key.This is **done to prevent an attacker impersonating a server**, which would give them the opportunity to capture your password or the contents of your session. Once you have verified the server’s key, it is recorded by the client in ~/.ssh/known\_hosts so it can be automatically checked upon each connection. 
3. You should be on the ssh server now.

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/sshoverview/ssh6.png)

#### Using Password based authentication:

Ssh'ing to a server using public/private key is a preffered way, but using Password based authentication to SSH to a server is also prefered. Here are the steps:

**Configure password login on  SSH server in sshd\_config:**

You will need to change the configuration of your ssh server. The file to be edited is `sshd_config`.

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/sshoverview/ssh7.png)

These are the important parameters you should be aware of:

### Initial server key discovery

### Executing commands remotely

### Redirecting commands’ input and output

### SSH forwarding

