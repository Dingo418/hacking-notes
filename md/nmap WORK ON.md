NMAP
What is NMAP
- Shows what a network is like:
-ports open
- type of connections
- Computers have a total of 65535 available ports
- 1024 of theses are “well known”

Important flags


switch
Example
Description
-sS
nmap 192.168.1.1 -sS
TCP SYN port scan (Default)
-sT
nmap 192.168.1.1 -sT
TCP connect port scan
(Default without root privilege)
-sU
nmap 192.168.1.1 -sU
UDP port scan
-sA
nmap 192.168.1.1 -sA
TCP ACK port scan
-sW
nmap 192.168.1.1 -sW
TCP Window port scan
-sM
nmap 192.168.1.1 -sM
TCP Maimon port scan

Different type of scans:

Tcp Scan: Very loud scan and can easily be detected as they can log ip’s.
UDP Scan: used to check if any UDP port is up. 
-Slow Scan
-possible false postives as no response

SYN Scan: Is a TCP scan but only sends the first packet to establish a TCP connection

ACK Scan:  Determines iof particular port is filtered or not
-Helpful for probing firewalls and exsiting rules
- Does not well agaisnt sophisticated firewalls

-FIN Scan: like SYN Scan but sends a TCP FIN packet instead.
- stealthy
- Can show False postives and negatives

NULL Scans: Set all headier fields to null
- Extremely Stealthy
- Unreliable on some versions of windows

XMAS Scan: manipulatse the PSH, URG and FIN flags in the TCP header.
- Stealthy
- Computers running windows will not be able to respond

RPC Scan: Used to discover machines that respond to Remote Procedure Call service
- Allows commands to run on certain m,achines remotly

IDLE Scan:Packets are bounced of external host
- Only used ,alicously
- Stealthiest



