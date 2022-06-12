
# Hashcat
https://hashcat.net/wiki/doku.php?id=example_hashes
``hashcat -m 1800 -a 0 hash.txt /usr/share/wordlists/rockyou.txt```

# John The Ripper
```
john [options] [path to file]

john - Invokes the John the Ripper program

[path to file] - The file containing the hash you're trying to crack, if it's in the same directory you won't need to name a path, just the file.

```
--format=[format]
john --list=formats | grep -iF "md5"
```
identifying hashes
``` 
wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py 

Then use:
```
john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt
```

##  Single Crack Mode
Single cracking is an implementation of word mangaling for common inserts of symbols like joker to j0ker
```
john --single --format=[format] [path to file]

--single - This flag lets john know you want to use the single hash cracking mode.
```

## Cracking linux passwords
```
unshadow [path to passwd] [path to shadow]

unshadow - Invokes the unshadow tool

[path to passwd] - The file that contains the copy of the /etc/passwd file you've taken from the target machine

[path to shadow] - The file that contains the copy of the /etc/shadow file you've taken from the target machine 
```

### Example

FILE 1 - local_passwd

Contains the /etc/passwd line for the root user:
```
root:x:0:0::/root:/bin/bash
```
FILE 2 - local_shadow

Contains the /etc/shadow line for the root user:
```
root:$6$2nwjN454g.dv4HN/$m9Z/r2xVfweYVkrr.v5Ft8Ws3/YYksfNwq96UL1FX0OJjY1L6l.DS3KEVsZ9rOVLB/ldTeEL/OIhJZ4GMFMGA0:18576::::::
```
## Other formats to crack
When cracking other formats you do not need to specify the format. Just use:
```
john --wordlist=/usr/share/wordlists/rockyou.txt ssh.txt
```
### Zip2John
```
zip2john [options] [zip file] > [output file]
```
``[options] `` Allows you to pass specific checksum options to zip2john, this shouldn't often be necessary

``` [zip file]``` - The path to the zip file you wish to get the hash of

``>`` - This is the output director, we're using this to send the output from this file to the...

```[output file]`` - This is the file that will store the output from

Example Usage
```
zip2john zipfile.zip > zip_hash.txt
```

### Rar2John
```
rar2john [rar file] > [output file]
```
Note you might need to install unrar to open the rar file once you have the password


### SSh2John
```
 ssh2john [id_rsa private key file] > [output file] 
 ```