# Metasploit
Metasploit is a widelt used expoitation framework. It has two main versions:
- Metasploit Pro: Made for the automation and managment of task with a GUI
- Metasploit Freamework: Open-source command line version.

The Main components of metasploits are:
- msfconsole: the command-line interface
- Modules: Supported extra bits of software like exploits, scanners and payloads.
- Tools: Stand alone tools.
## Main components of metasploit
Some prequiste knowledge
- exploit: A piece of code that uses a vulnerability present on the target system.
- Vulnerability: A design, coding, or logic flaw affecting the target system. The exploitation of a vulnerability can result in disclosing confidential information or allowing the attacker to execute code on the target system.
- Payload: An exploit will take advantage of a vulnerability. However, if we want the exploit to have the result we want (gaining access to the target system, read confidential information, etc.), we need to use a payload. Payloads are the code that will run on the target system.

The modules:
### Auxiliary: 
Any supporting module, such as scanners, crawlers and fuzzers, can be found here. 
### Encoders: 
Encoders will allow you to encode the exploit and payload in the hope that a signature-based antivirus solution may miss them. 
### Evasion: 
While encoders will encode the payload, they should not be considered a direct attempt to evade antivirus software. 
### Exploits: 
Exploits, neatly organized by target system.
### NOPs
NOPs (No OPeration) do nothing, literally. 
### Payloads
Payloads are codes that will run on the target system. 
- Singles: Self-contained payloads (add user, launch notepad.exe, etc.) that do not need to download an additional component to run.
- Stagers: Responsible for setting up a connection channel between Metasploit and the target system. Useful when working with staged payloads. “Staged payloads” will first upload a stager on the target system then download the rest of the payload (stage). This provides some advantages as the initial size of the payload will be relatively small compared to the full payload sent at once.
- Stages: Downloaded by the stager. This will allow you to use larger sized payloads.

Metasploit has a special way of telling you whether your payload is a staged or stagless pauyloads.         ◦ Stageless payloads are denoted with a (_). eg
- generic/shell_reverse_tcp
- linux/shell_bind_tcp
Staged has a / instead of a "_"
- generic/shell/reverse_tcp
- linux/shell/bind_tcp

### Post
Post modules will be useful on the final stage of the penetration testing process listed above, post-exploitation.
### Note
You can view all these under /usr/share/metasploit-framework/modules/payloads/ on the default version of kali.

## msfconsole
This is the default exploit maker. You can run by using "msfconsole".

### Useful commands:
It supports most basic linux commands nad aut completion.
- ls: lists the contents of the current directory you are in
- ping: pings things
- help set: help menu for specfic thing
- history: figure it own einstein
- search PARAM: You can conduct searches using CVE numbers, exploit names or target systems. it also gives you irs rank.
- background: use this command to set the session prompt to the background
- sessions: lists the sessions. If you want to interact with them use "-i". eg. sessions  -i 1.
  
#### Useful commands for setting up hacks  
1. use exploit/windows/smb/ms17_010_eternalblue: uses exploit
- show options: shows the options
- show payloads: shows the payloads
- set: sets options, use info to see if required.
- setg: sets options universally across scripts
- unset: unsets variable
- unsetg: unsets variable universally
- exploit: it launches the command. If you add "-z" it will run in the background
- check: some modules support check option that will check if system is vulnerable.
- back: goes back
- show options: shows info about optionbs
- info: info about the payload

## Examples

### Port scanning
- using metasploit packages
  1. search portscan
  2. use scanner/portscan/tcp
  3. show options
  4. set RHOSTS IP
  5. exploit
- Using nmap
  1. sudo nmap -sS 10.10.244.140

**UDP service Identtification:**
use scanner/discovery/udp_sweep: allow you to quickly identify services running over the UDP

**SMB Scans:**
use either smb_enumshares or smb_version


PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
8000/tcp open  http-alt