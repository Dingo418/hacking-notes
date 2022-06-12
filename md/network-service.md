# Network Servies 1

## SMB

### What is SMB?
SMB - Server Message Block Protocol - is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network. [source]
Servers make file systems and other resources (printers, named pipes, APIs) available to clients on the network. Client computers may have their own hard disks, but they also want access to the shared file systems and printers on the servers. The SMB protocol is known as a response-request protocol

### What runs SMB?
Microsoft Windows operating systems since Windows 95 have included client and server SMB protocol support. Samba, an open source server that supports the SMB protocol, was released for Unix systems.
Acessing SMB
smbclient //[IP]/[SHARE] 
Followed by the tags:
-U [name] : to specify the user
-p [port] : to specify the port

![SMB](img/SMB_Graphic.png)

### Enumeration
1. Do a port scan
2. Enum4Linux
Enum4linux is a tool used to enumerate SMB shares on both Windows and Linux systems. It is basically a wrapper around the tools in the Samba package and makes it easy to quickly extract information from the target pertaining to SMB. It's installed by default on Parrot and Kali, however if you need to install it, you can do so from the official github. 
The syntax of Enum4Linux is nice and simple: "enum4linux [options(-a)] ip"

## Telnet

### What is it?
Is a application protocol that conects and excutes commands on rmote machines. This is all done by clear text
How to use it:
You can connect to a telnet server with the following syntax: "telnet [ip] [port]"
In the ftp service use get to get a file
## FTP

### What is FTP?
File Transfer Protocol (FTP) is, as the name suggests , a protocol used to allow remote transfer of files over a network
### How does it work?
A typical FTP session operates using two channels: 
    • a command (sometimes called the control) channel 
    • a data channel. 
### Active vs Passive
The FTP server may support either Active or Passive connections, or both.  
    • In an Active FTP connection, the client opens a port and listens. The server is required to actively connect to it.  
    • In a Passive FTP connection, the server opens a port and listens (passively) and the client connects to it.  
## Hydra
### How to use:
"hydra -t 4 -l dale -P /usr/share/wordlists/rockyou.txt -vV 10.10.10.6 ftp"



# Network Services 2
## NFS
### What is NFS:
NFS stands for "Network File System" and allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files. It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.
### Acessing NFS
If someone wants to access a file using NFS, an RPC call is placed to NFSD (the NFS daemon) on the server. This call takes parameters such as:
    •  The file handle
    •  The name of the file to be accessed
    •  The user's, user ID
    •  The user's group ID
### Mounting NFS
sudo mount -t nfs IP:share /tmp/mount/ -nolock
Let's break this down



|Tag | Function |
| ---| --------|
|sudo | Run as root|
|mount | Execute the mount command|
|-t nfs | Type of device to mount, then specifying that it's NFS|
|IP:share | The IP Address of the NFS server, and the name of the share we wish to mount|
|-nolock | Specifies not to use NLM locking|





### Exploiting NFS
You upload a payload through nfs with the right ownership and permissions then you run it through ssh. 
    • Chmod +rsw bash
    • chown root:root bash

## SMTP:
### Urls:
https://tryhackme.com/room/networkservices2
https://www.afternerd.com/blog/smtp/
https://computer.howstuffworks.com/e-mail-messaging/email3.htm
### Understanding SMTP:
    • Stands for “Simple Mail Transfer Protocol”
    • Can work on both linux and windows
    • Preforms 3 functions
        ◦ It verifies who is sending emails through the SMTP server.
        ◦  It sends the outgoing mail
        ◦  If the outgoing mail can't be delivered it sends the message back to the sender
### POP (Post office Protocol)
    • POP's more simplistic approach of downloading the inbox from the mail server, to the client
### IMAP (Internet Message Access Protocol)
    • IMAP will synchronise the current inbox, with new mail on the server, downloading anything new
    • Changes over one computer will sync with multiple computers
### Enumerating SMTP:
    1. Run a port scan (nmap -T4 -p- <ip of vm>)
    2. Start up metasploit
    3. Use the  auxiliary/scanner/smtp/smtp_version for information
    4. Select Rhosts
    5. Use auxiliary/scanner/smtp/smtp_enum to find users
    6. use wordlist (/usr/share/seclists/Usernames/tio0usernames-shortlist.txt)


### Exploting SMTP
    1. Use hydra
    2. "hydra -t 16 -l USERNAME -P /usr/share/wordlists/rockyou.txt -vV 10.10.44.190 ssh"
|SECTION | FUNCTION|
| -------| --------|
|hydra | Runs the hydra tool
|-t 16 | Number of parallel connections per target
|-l [user] | Points to the user who's account you're trying to compromise
|-P [path to dictionary] | Points to the file containing the list of possible passwords
|-vV  | Sets verbose mode to very verbose, shows thelogin+pass combination for each attempt|
|[machine IP] | The IP address of he target machine|
|ssh / protocol |Sets the protocol|

## MySQL
### Understanding:
- The server handles all database instructions like creating, editing, and accessing data. It takes and manages these requests and communicates using the MySQL protocol. 
- MySQL is a relational database management system (RDBMS) based on Structured Query Language (SQ)
- The server handles all database instructions like creating, editing, and accessing data. It takes and manages these requests and communicates using the MySQL protocol. 
- Use for back end database

### Acessing database:
    1. MySQL creates a database for storing and manipulating data, defining the relationship of each table.
    2. Clients make requests by making specific statements in SQL.
    3. The server will respond to the client with whatever information has been requested.
### Enumerating MySQL
    1. Scan using nmap by nmap --script=mysql-enum <target> or nmap -A –p- -T5 Pn <ip>
    2. use metasploit mysql_sql module.
    3.  Put default ip’s in
    4. then change select version() to show databases
### Exploting MySQL
    1. Use module auxiliary/scanner/mysql/mysql_schemadump
    2. use module auxiliary/scanner/mysql/mysql_hashdump
    3. break the hash with jack the ripper
