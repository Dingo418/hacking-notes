# Upload Vulnerabilites

## Overwriting Existing files
Sometimes you can overwrite a pciture that is displayed on a wepage with your own if it has the same name as it. You can check it in the element view.

## Checking for pages
Use something called gobuster.
```
gobuster dir -u shell.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```


## Remote Code Execution

Sometimes you can causse your script to run. either using a webshell or reverse shell. An example of a basic web shell is.
```
<?php
    echo system($_GET["cmd"]);
?>
```
## Filtering
There are two main types of filtering. Client side and Server side.
- On client filters the web application filters it. It makes it very easy to bypass
- On server filters it is harder to bypass as you don't know what the actual code is an you can't just skip what you want.

#### Extension Validation
Blacklist or whitlists extensions. An example would be only accepting txt files. Very easy to bypass.

#### Filter type Filtering
- Mime validation:  MIME (Multipurpose Internet Mail Extension) types are used as an identifier for files.
![MIME](/img/MIME.png)
- Magic Number Validation: Most formats have a format number like, .PNG, and is a more accurate way to see what the file is.
![Magic Numbers](/img/magic-numbers.png)

- File length filtering
- File Name filtering: get rid of charaters such as ; or " ".
- File Content Filtering: Scans whole file to make surte it is not spoofing.

#### File Extensions
When the server is using a blacklist then sometimes they haven't blocked all the offshoots of a specfic language. For example php can be:
- php3
- php4
- php5
- php7
- phps
- php-s
- pht 
- phar
Also sometimes they only check the first or last file extension so you can do shell.jpg.php.

#### Magic Numbers
Sometimes when detecting files thye look inside the file into something called a magic number. These are generally the first 4 hexadecmils. You can change it using a hex editor. to find all the signautre look here. https://en.wikipedia.org/wiki/List_of_file_signatures

#### Hacking into a server (methodolgy)
1. Find info about a serveer using headers, using burpsuite, or the wappalzer extension. You can also look at the html code. Also make sure your server can run the code. use this https://github.com/swisskyrepo/PayloadsAllTheThings
2. Find an upload page, when editing it using burpsuites sometimes javascript won't show, this is due to your browser caching the javascript. You can reset this by ctrl + f5
3. Attempt to upload a completly innocent file. If they do not allow you to see the directory, make sure to use geobuster.
- make sure to use -x switch
4. acess or excute the file

If their is server-side filtering you can test by using
- uploading a file with a bad extension, eg. abc.testingextension
- Change magic number on something you know is good to see if their is magic number filtering
- Progressivly upload bigger files untill you hit a limite
- Intercept request with burpsuite and change the MIME type of the upload.
  
