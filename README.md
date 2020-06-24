# Presentation
In this repository, we're going to enounce the process needed to enable SSH through Windows, Mac and Linux.

SSH or Secure Shell is a protocol connecting one computer to another through command line. In my personal case, it's very useful through system administration in order to update softwares, manage configuration and deployment. SSH is working on Linux & Mac, but we'll see alternatives that let us use SSH on windows. SSH runs on TCP port 22.

SSH connection between a client and a server is encrypted and more secure than other connection protocol like FTP or Telnet.

# Prerequisites

## Clients

To use SSH, you have many clients or softwares which allow you to set SSH connections from a machine to a SSH server:

- On Mac OS & Linux: it's already built-in, you don't need to install a client.
- On Windows: you have Putty or MobaXterm.
- On Android: JuiceSSH.

Also, verify that the SSH server is running on the machine you need to connect to.

## If you use VM

If you need to connect to a VM through SSH, set your network interface of your VM on "bridge". It will let your VM appear like a any other machine on the network with its own IP address. It will also be easier to connect through SSH.

## If you use Vagrant

[...]

## Make life easier

You can register the IP address of the SSH server machine on the known hosts. You can avoid entering the IP address for each connection and just enter the name of the machine for example. On windows, you can generally find the hosts file through the path:

>C:/Windows/System32/Drivers/etc/hosts

Modify the hosts file and specify the IP address and the name of the machine.

## Two type of authentication to know

You have two possibilities of authentication on a SSH server machine through SSH:

- With the username + password, 
- With a cryptographic key pair generated and shared with the SSH server.

# Connecting securely with a key pair

## The principle

To access to a SSH server, the use of a cryptographic key pair is more secure rather than a username and password. Key-based access requires a cryptographic key pair with a public key and private key:
- The SSH server holds the public key, 
- We keep the private key.

## Generate a key pair and send the public key to the SSH server

- On Windows (with Cygwin or Puttygen):

>Launch puttygen,

>Generate a public/private key pair,

>Set a passphrase (in the future to unlock the key),

>Save the public key and the private key on the host,

>Copy the public key from PuttyGen,

>Go to the SSH server and past the public key in ".ssh/authorized_keys" (if the folder .ssh and the file authorized_keys don't exit, you have to create it before "~/.ssh/authorized_keys").

- On Mac & Linux: 

>Open a terminal and type "ssh-keygen -t -rsa" (RSA is the encryption algorithm): it will generate a public/private rsa key pair,

>Enter the file in which you want to save the key (enter the path+filename): for example, the public key will be "key.pub" and the private key will be "key",

>Set a passphrase for security, 

>Copy the public key into the SSH server with the command "scp /path-to-the-public-key/ user@IPaddress:~/" or copy it manually,

>If you copied the public key with the scp command, on the SSH server you will probably have to concatenate the public key into "~/.ssh/authorized_keys" file (if the file and the folder don't exist, you have to create it before).

>To connect to an SSH server from Mac OS X (or Linux), you have to specify the path to the private key by typing the command: "ssh -i /path-to-the-private-key/ username@IPaddress" (you should be able to connect to the SSH server).

>Now if you want to ease your life and avoid typing this long command, you can create a quick shortcut through the config file (~/.ssh/config), for example:

```
Host 192.168.50.4
  User username
  IdentityFile /path-to-the-private-key/
```

>Now with "ssh IPaddress", you can directly connect to the SSH server.

# Reference

Learning SSH through LinkedIn Learning : https://www.linkedin.com/learning/learning-ssh/generating-a-key-pair-on-windows?u=2006794