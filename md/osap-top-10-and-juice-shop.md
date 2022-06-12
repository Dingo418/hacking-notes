
# OWASP Top 10
## Injection
When user controlled inputs are interpreted as actual commands.
Some common injection examples are
- SQL Injection: Is a user controlled input passed to SQL queries.
- Command Injection:is when a user input is passed to system commands.
  
If an attacker is able to passs input into interpreter then you can,
- Access, Modify and Delete information in a database when this input is passed into database queries
- Execute Arbitrary system commands on a server that would allow an attacker to gain access to users’ systems

To defend agaisnt this make sure to:
- Use a allow list: compare input to list of allowed charaters
- To strap an input: if their is dangerous charaters then remove them


## Broken Authentication
If you can find a way to exploit a broken authentication mechanism then you can gain access to web applications.
There are 3 main ways to break it.
- Brute Force Attacks: brute force passwords and usernames
- Weak Credentials: Guess passwords like password1
- Weak Session Cookies: Session cookies keep track of who you are as they are a statless protocol. If they contain predictable values you can guess them.

To Avoid:
- Ensure a strong password policy
- Add an automated lockout
- Implement multi factor authentication

### Simple exploting
You might be able to create the same account using theses methods.
- " bob" == "bob"
- '"admin"' == 'admin'

## Senstive Data Exposure
Could accidently divulge senstive information like names, date-of-birth, finical info and with advanced techniques possibly even passwords and usernames. Using advancded techniques you can use man in middle and force user connections through compromised device which you can then break encrypton with.

### SQL
Their is two version of databases. A full server or a flat-file which contains all info in a file.
You can find alot of compromising data but the ones that are most interesting is password hashes

To access the database use:
1. sqlite3 database-name
2. We can see the table by .tables
3. Use PRAGMA table_info(customers); to gain more information
4. Use SELECT * FROM customers; to access the hashes

### Password hashes
You can crack password hashes using many tools. A website is https://crackstation.net/.

## xml
XML External Entity (XXE) is a vulnerability that abuses the feature of XML parsers and data. It allows the attacker to intera ct with the backendo r xternal system. This can lead to DoS attacks or Server Side Request Forgery. In extreme cases it can enable port sanning and led to remote code excution

Two main types of XXE attacks:
- In-Band: attacker recevies immediate response
- out-of-band: no immediate response, has to reflec ttheir ouput on some other file or their own server

### Payload
payload 1
```<!DOCTYPE replace [<!ENTITY name "feast"> ]>
 <userInfo>
  <firstName>falcon</firstName>
  <lastName>&name;</lastName>
 </userInfo>
```

```
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
<root>&read;</root>
```

If the website is vulnerable then you can use this to read the contents of ```/etc/passwd```
Once you got their password you can go to their ssh key and read it

## Broken Access control

Scenario #1: The application uses unverified data in a SQL call that is accessing account information:
pstmt.setString(1, request.getParameter("acct"));
ResultSet results = pstmt.executeQuery( );

An attacker simply modifies the ‘acct’ parameter in the browser to send whatever account number they want. If not properly verified, the attacker can access any user’s account.
http://example.com/app/accountInfo?acct=notmyacct

Scenario #2: An attacker simply force browses to target URLs. Admin rights are required for access to the admin page.
http://example.com/app/getappInfo
http://example.com/app/admin_getappInfo

If an unauthenticated user can access either page, it’s a flaw. If a non-admin can access the admin page, this is a flaw (reference to scenarios).

## Security Misconfiguration
Literally just default credentials


## Cross Site Scripting
A web application is vulnerable to XSS if it uses unsanitized user input. XSS is possible in Javascript, VBScript, Flash and CSS. There are three main types of cross-site scripting:
- Stored XSS - the most dangerous type of XSS. This is where a malicious string originates from the website’s database. This often happens when a website allows user input that is not sanitised (remove the "bad parts" of a users input) when inserted into the database.
- Reflected XSS - the malicious payload is part of the victims request to the website. The website includes this payload in response back to the user. To summarise, an attacker needs to trick a victim into clicking a URL to execute their malicious payload.
- DOM-Based XSS - DOM stands for Document Object Model and is a programming interface for HTML and XML documents. It represents the page so that programs can change the document structure, style and content. A web page is a document and this document can be either displayed in the browser window or as the HTML source.

basic examples:
- Popup's (```<script>alert(“Hello World”)</script>```) - Creates a Hello World message popup on a users browser.
    Writing HTML (document.write) - Override the website's HTML to add your own (essentially defacing the entire page).
- XSS Keylogger (http://www.xss-payloads.com/payloads/scripts/simplekeylogger.js.html) - You can log all keystrokes of a user, capturing their password and other sensitive information they type into the webpage.
- Port scanning (http://www.xss-payloads.com/payloads/scripts/portscanapi.js.html) - A mini local port scanner (more information on this is covered in the TryHackMe XSS room).
- Finding your IP: ```<script>alert(window.location.hostname)</script>```
- Defacing websites: ```<script>document.querySelector('#thm-title').textContent = 'I am a hacker'</script>```


## Insecure deserilization
Best to look on the website, pretty complicated. This definition is still quite broad to say the least. Simply, insecure deserialization is replacing data processed by an application with malicious code; allowing anything from DoS (Denial of Service) to RCE (Remote Code Execution) that the attacker can use to gain a foothold in a pentesting scenario. Essentially exploiting cookies. you can change them. Use burp suite decoder for decoding base 64. if you have encoded payload you can make a reverse shell.


## Componenets with known Vulnerabilites
Pretty simple, company uses outdated software that has a vulnerability in it. Just use exploitDB with it


## Logs
logs mmm



# OWASP-Juice-Shop

## SQL INJECTION
if you change the field of {"email":"bender@juice-sh.op", "password":"a"} to {"email":"bender@juice-sh.op'--", "password":"a"} then it auto logs you in. This is due to Well, as the email address is valid (which will return true), we do not need to force it to be true. Thus we are able to use '-- to bypass the login system. Note the 1=1 can be used when the email or username is not known or invalid.


## NULL TERMINATOR
By add %2500.md at the end of the url allows us to bypass the pdf thing

## source code
You can see if their is a path to a administrator page

# XSS

DOM (Special)
	DOM XSS (Document Object Model-based Cross-site Scripting) uses the HTML environment to execute malicious javascript. This type of attack commonly uses the <script></script> HTML tag.
Persistent (Server-side)
	Persistent XSS is javascript that is run when the server loads the page containing it. These can occur when the server does not sanitise the user data when it is uploaded to a page. These are commonly found on blog posts. 
Reflected (Client-side)
	Reflected XSS is javascript that is run on the client-side end of the web application. These are most commonly found when the server doesn't sanitise search data. 

eg.
```<iframe src="javascript:alert(`xss`)"> ```

