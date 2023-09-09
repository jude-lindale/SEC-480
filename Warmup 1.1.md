# Summary
**Tester:** Jude Lindale

**Target:** bree.shire.org

| IP Addresses |  Ports  |
| ------------ | ------- |
|  10.0.5.32   | TCP: 22, 80|

**Description:** The test on bree.shire.org conducted by Jude Lindale resulted on the exploitation of Cockpit which allowed the tester to reset admin passwords for the Cockpit allowing access to the Cockpit user interface, as well as remote code execution through a reverse shell on the machine hosting Cockpit which resulted in the tester to escalate their privilege to a root user and obtaining both the root and user flags.


## Target Overview
bree.shire.org is a Ubuntu Linux system running OpenSSH 7.6p1 on port 22 and Apache httpd 2.4.29 on port 80 and is also hosting the cockpit content management system.

## Vulnerability Explanation
**1. Cockpit CMS 0.11.1 - Username Enumeration & Password Reset NoSQL Injection:** this is a vulnerability in the  /auth/resetpassword and /auth/newpassword directories that allows for the extraction of the password reset tokens allowing for user details enumeration and password reset through the use of sql.

**2. Cockpit CMS 0.6.1 - Remote Code Execution:** this vulnerability allows for an attacker to access a target system using this version of Cockpit and make changes, run commands, and exfiltrate data, among other things, remotely regardless of the target and attacking devies location.

### Vulnerability Fix: 
**1. Cockpit CMS 0.11.1 - Username Enumeration & Password Reset NoSQL Injection:** 1) update all systems running anything lower than the latest version. 2) Use of predefined statements that are allowed will eliminate to possibility of sql injection.

**2. Cockpit CMS 0.6.1 - Remote Code Execution:**  Updating and patching to the newest version of Cockpit should be done on all systems running Cockpit.

Severity: Critical

## Supporting Evidence

### Scanning and Enumeration
The enumeration portion of this penetration test focuses on gathering information about what services are up on the target system. This is valuable information as it provides detailes on potential attack vectors into a system. Understanding what applications are running on the system gives the needed information before performing the actual penetration test.

Enumeration of the target bree.shire.org, using nslookup, nmap, dirb, and interacting with the webapp provided the following information. 

Show in figure 1.1 the nslookup results provid the IP address tied to bree.shire.org, 10.0.5.32. Furthermore, figures 1.2 shows port 22 is open running OpenSSH 7.6p1 on Ubuntu and port 80 is open running Apache httpd 2.4.29 on Ubuntu. With firgure 1.3 showing the results of the nmap vulnerability scan showing potential vulnerable points to be used to gain access. Shown in figure 1.4 are the dirb scan results with show the web applications sub-paths.

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/84e8ef76-8a30-4106-9f5e-a1e84b1e1463)

*Figure 1.1: nslookup Results*

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/38b05aff-a16f-4c69-8b4d-5201172bb2a3)

*Figure 1.2: Nmap Scan Results*

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/45ec6e02-88e7-4c21-a93f-39b10b107421)

*Figure 1.3: Nmap Vulnerabulity Scan Results*

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/364635f2-0449-4437-91df-7776a6d17171)

*Figure 1.4: Dirb Scan Results*

Figure 2.1 shows the landing page of the web application when using the IP address. However by using the paths that were found through dirb we are able to see the web applications file directory as seen in figures 2.2 and 2.3.

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/1d027728-5c29-4a06-aad4-7e5d308c61a2)

*Figure 2.1: Webapp interaction Results*

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/715d8acd-646c-4247-9ef6-3413c3e2b866)

*Figure 2.2: Webapp Interaction Results*

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/74676f90-a543-4839-a968-d8dbe35427e3)

*Figure 2.3: Webapp Interaction Results*

### Vulnerability Detection
With the obtained results from the enumeration stage and interacting with the webapp the information that was found provided possible attack vectores which were confirmed through the use of searchsploit, and searching for vulnerabilities for cockpit. Finding two vulnerabilities in particular as shown in figure 3.1, 3.2, and 3.3:

Cockpit CMS 0.11.1 - Username Enumeration & Password Reset NoSQL Injection (CVE-2020-35847, CVE-2020-35848)
Cockpit CMS 0.6.1 - Remote Code Execution

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/3d375ce7-983e-40fc-bad0-a94cef351d8e)

*Figure 3.1: Vulnerability Scan Results*

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/d3232bd8-1df1-4ca6-b7a8-920357eac5ca)

*Figure 3.2: Searchsploit Result*

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/957cc8c8-013b-40de-9a62-b9971f0ce62e)


*Figure 3.3: Searchsploit Result*


### Foothold
To access the Cockpit login page the first step is to change host file to include the IP address, 10.0.5.32, and the URL, bree.shire.org. As seen below in figure 4.1. 

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/b0dfe298-d411-4bdf-8d71-222f8a493795)

*Figure 4.1: Change Host File*

With the change made to the host file it is now possible to navigate to the Cockpit login page shown in figure 4.2.

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/c92688d3-f9eb-4b63-bcf9-3f33250cab1f)

*Figure 4.2: Cockpit Login Page*

By using the inspector tool within the FireFox web browser the exact version of Cockpit can be found to as 0.50  shown in figure 4.3.

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/9a614168-d904-4992-8c15-52d34fa92af5)

*Figure 4.3: Cockpit Webpage Inspector Tool*

Cockpit can be exploited by chaining togeather the Username Enumeration & Password Reset NoSQL Injection vulnerability and Remote Code Execution. 

By using the path in Figure 3.2 is the exploitation file. With the use of python3 to execute the file pointing it at bree.shire.org, Users are Found for admin, barliman, and strider as shown in figure 4.4 barliman is a part of the admin group this means that this user has the as privileges as the admin account. By choosing barliman as the user, the python script then with reset the user account password.

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/81ac5506-df96-4217-97ef-135ced03a69d)

*Figure 4.4: Usernames and Passwords*

With the barliman's password changed remote code execution is achievable. A Weevely instance is generated to be uploaded to the target host. The creation of the Weevely instance is shown in figure 4.5.

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/85d5ac00-d99b-4d11-9c18-a676b8c696b3)

*Figure 4.5: Weevely Instance Creation*

With the Weevely instance file created the next step is to start an http server using python running on port 8080 as shown in figure 4.6.

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/5fc8deb4-5448-44f9-a330-2b0aa15a1a4b)

*Figure 4.6: Start HTTP Server Instance*

The command format uses the curl command 1. passing a Content-Type header to tell the target API that the command is sending a JSON payload with -H ‘Content-Type: application/json’, 2. Sending an object with --data-raw "{\"email\":\"$EMAIL\",\"otp\":\"$OTP\", \"password\":\"$NEW_PWD\"}", 3. The url to send it to, and 4. Output the results into a file called bree.html. Using a variation of this command, the Weevely instance file judebree.php can uploaded to the target http://bree.shire.org/auth/check by using the POST method and retrieving the Weevely instance file from the http server in figure 4.6. shown in figure 4.7.


![image](https://github.com/jude-lindale/SEC-480/assets/70959569/86e58692-76d4-4949-9218-c6c1d664b4a4)

*Figure 4.7: Weevely Instance Upload*

Weevely is used by pointing it to the instance file location on bree.shire.org a shell is achieved shown in figures 4.8 and 4.9.  

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/ac7c9b6f-3f09-4fbf-8599-6d35d44d4ba8)

*Figure 4.8: Weevely Shell*

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/73899eda-8727-49b8-b4f9-134ffea3797c)

*Figure 4.9: Weevely Shell*

Netcat on the attacking host to create a listener, shown in figure 4.10, allows for the creation of a revershell.

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/c774b175-2987-4c60-a727-330c4dafd6dd)

*Figure 4.10: netcat listener*

Running the command shown in figure 4.11, points the shell instance to the attacking host netcat listener, providing the host IP address and the port the listener is on a reverse shell, creating a foothold.

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/d76ef6f7-88fa-4cc7-9267-1090a4dc4254)

*Figure 4.13: Reverse Shell Command*

### Privilege Escalation
Within the home directory there are three user accounts: barliman, deployer, and samwise. Samwise is an account that has previously been cracked and with the password Mallorn79. With the access to the samwise account root privilege escalation is possible as shown in figures 5.1 and 5.2.

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/a00e0129-e705-49f3-abf7-d9ad4f8d57b6)

*Figure 5.1: Privilege Escalation to samwise*

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/eafa5ca8-2065-46c1-921d-d68f5e412703)

*Figure 5.2: Privilege Escalation to root*

### Post Exploitation 
With root privileges both root and user flags are obtained shown in figure6.1 

![image](https://github.com/jude-lindale/SEC-480/assets/70959569/684a23cb-4120-4bb8-88b6-46ee4143d8ef)

*Figure 6.1: Obtaining User and Root Flags*

### Clean Up
In the case the only thing the needed to be done is the deletion of the Weevely instance file located at http://bree.shire.org/breejude.php and close all connections.
