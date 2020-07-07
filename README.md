# Presentation
In this repository, we're going to enounce the process needed to enable SSH through Windows, Mac and Linux.

SSH or Secure Shell is a protocol connecting one computer to another through command line. In my personal case, it's very useful through system administration in order to update softwares, manage configuration and deployment. SSH is working on Linux & Mac, but we'll see alternatives that let us use SSH on windows. SSH runs on TCP port 22.

SSH connection between a client and a server is encrypted and more secure than other connection protocol like FTP or Telnet.

# Prerequisites

## Clients to connect to the SSH server

To use SSH, you have many clients or softwares which allow you to set SSH connections from a machine to a SSH server:

- On Mac OS and Linux: it's already built-in, you don't need to install a client.
- On Windows: you have Putty or MobaXterm.
- On Android: JuiceSSH.

Also, verify that the SSH server is running on the machine you need to connect to.

## Setting up an SSH server

- On Linux, you can do it through the following steps:
> sudo apt-get openssh-server (apt for Debian (Ubuntu, Mint) and yum for RedHat (CentOS, Fedora))

Then you can start the SSH service. You can also modify the SSH configuration file accessible at "/etc/ssh/sshd_config":
> "PasswordAuthentication yes" (to connect with username+password, if you put no you'll necessarily need a key pair)

- On Windows, consider first an other OS, it would be better. SSH isn't native to Windows environment. To connect remotely to Windows machines, consider RemoteDesktop. If it's not possible you have a solution through Cygwin, go check [my repository on the subject](https://github.com/liquid4teur/Cygwin).

## If you use VM

If you need to connect to a VM through SSH, set your network interface of your VM on "bridge". It will let your VM appear like a any other machine on the network with its own IP address. It will also be easier to connect through SSH.

## If you use Vagrant

Vagrant set up automatically the SSH connection between your host and the vagrant box. To connect, you just have to be inside the vagrant environment folder and use the command:
>vagrant ssh

During the setup of a vagrant box, the private key is set on the host (in the .vagrant folder) and the public key is set on the vagrant box. If you want to know more about Vagrant, go check my [Vagrant repository](https://github.com/liquid4teur/Vagrant).

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

>Now open Putty to setup an SSH connection by putting the username@IP_address (akram@192.168.00.00),

>Go to "SSH" category, then in "Auth",

>Browse and select the private key (corresponding to the public key) for the authentication and click open, 

>It will ask for the passphrase (if there is one) and now it's connecting using the certificate.

- On Mac & Linux: 

>Open a terminal and type "ssh-keygen" (RSA is the encryption algorithm): it will generate a public/private rsa key pair,

>Enter the file in which you want to save the key (enter the path+filename): for example, the public key will be "key.pub" and the private key will be "key",

>Set a passphrase for security, 

>Copy the public key into the SSH server with the command "scp /path-to-the-public-key/ user@IPaddress:~/" or copy it manually,

>If you copied the public key with the scp command, on the SSH server you will probably have to concatenate the public key into "~/.ssh/authorized_keys" file (if the file and the folder don't exist, you have to create it before) with the command: "cat key.pub > .ssh/authorized_keys" (you'll probably have to be root in order to create this folder).

>To connect to an SSH server from Mac OS X (or Linux), you have to specify the path to the private key by typing the command: "ssh -i /path-to-the-private-key/ username@IPaddress" (you should be able to connect to the SSH server).

>Now if you want to ease your life and avoid typing this long command, you can create a quick shortcut through the config file (~/.ssh/config), for example:

```
Host 192.168.50.4
  User username
  IdentityFile /path-to-the-private-key/
```

>Now with "ssh IPaddress", you can directly connect to the SSH server.

# Transfering files via SSH

## SFTP (SSH File Transfer Protocol)

The operation through SFTP is different than FTP. SFTP is encrypted and works on only one port, FTP isn't encrypted and can work through many ports.

## SFTP/FTP with clients

The most accessible one is FileZilla (works on Windows, Mac OS and Linux). To work with FileZilla, you have to:

- Put the host, the username and password, the port (22 for SSH) and do quick connect.
- On the right panel you'll find the remote host, you can select the files you want to transfer and drag them into the location you want (from the host to the local host and also in the other way).

## SFTP/FTP with CLI

It only works on Linux and Mac OS (it can work on Windows if you're using Cygwin). 

To launch an SFTP session, type the command:
> sftp username@ip-address 

With "?" you can list the commands available. Here some examples:

- If you are typing the command to know something on the local host, it will have this form:
> lcd (local host + cd)
> lls (local host + ls) 

- If you are typing the command to know something on the remote host, it will have this form:
> cd 
> ls

To transfer files from the host to the remote host: 
> put file-on-the-host path-on-remote-host

To exit the SFTP session, type:
> bye

## SCP 

If you need to quickly send or receive files to or from a server running SSH, you can use SCP to do so. SCP comes with Mac OS and Linux (also for Windows, we can let it work with Cygwin).

To use SCP, type the command (it will copy the file to the home server):
> scp path-of-the-file username@ip-address:

You can specify a path for the destination:
> scp path-of-the-file username@ip-address:/home/akram/Bureau

You can also receive from the remote host by doing the reverse:
> scp username@ip-address:/path-of-the-file-on-remote-host/ /path-on-the-host/

## SFTP vs SCP

- SCP is more faster than SFTP, 
- Yet, with SFTP you have less loss of data and more features through the variety of commands.

# SSH Tunnel 

## Presentation

An SSH Tunnel is a process allowing to encapsulate data from another protocol based on TCP/IP (like HTTP, IMAP, POP3, FTP, ..) into SSH. It works through forwarded ports, from a port on a local machine to a port on a remote machine (port 22 or other). 

## Local port forwarding

Through the terminal command line, you can type the command (for example with mySQL server):
> ssh -L 9000:127.0.0.1:3000

- 9000 is the port on the local machine
- 127.0.0.1 is the localhost where is running mySQL server
- 3000 is the port on the remote server

The connection to databases is also possible through an SSH tunnel.

# Reference

Learning SSH through LinkedIn Learning : https://www.linkedin.com/learning/learning-ssh/generating-a-key-pair-on-windows?u=2006794

Virtual Networking for VirtualBox : https://www.virtualbox.org/manual/ch06.html