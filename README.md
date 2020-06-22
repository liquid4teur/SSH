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

# Authentication

You have two possibilities of authentication on a SSH server machine through SSH:

- With the username + password, 
- With a cryptographic key pair generated and shared with the SSH server.