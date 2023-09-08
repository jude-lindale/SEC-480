
# hostname

| IP Addresses |  Ports  |
| ------------ | ------- |
|  10.0.5.27   | 22, 80, 3000|

## Target Overview
arwine.shire.org is a Ubuntu Linux system running OpenSSH 8.9p1 on port 22, Apache httpd 2.4.52 on port80, with another open port of 3000

## Vulnerability Explanation
1. The website contained an outdated version of gitea.  1.19.0 is the current version.  1.12.5 was loaded. 
   - Fix: Upgrading all applications to their current version is essential as each version patches known and exploitable vulnerabilities.
   - Severity: High

 2. Administrative credentials for the arwen user of gitea were posted on a the server and accessible via a web call to arwen.shire.org/admin. Passwords should not be stored in plain text and should never be stored without encryption.
    - Fix: Record passwords offline in a secure location (like a safe) or use an approved password management system on a separate workstation.
    - Severity: High

3. The arwen user employed a weak password for the linux systems (SecurePassworD).  
   - Fix: Policy should be changed to require longer, complex passwords for administrative access.  No dictionary words should be allowed. Ideally, multi-factor authentication should be required for all servers.
   - Severity: High


## Supporting Evidence

> :bulb: This is the place for screenshots and descriptions that illustrates the steps necessary to carry out the attack.  When possible output tabular data to a text file on your kali system for future analysis.

* Scanning and Enumeration
* ![image](https://github.com/jude-lindale/SEC-480/assets/70959569/03859616-f0e7-4397-85f0-6d03e9d9d389)
* ![image](https://github.com/jude-lindale/SEC-480/assets/70959569/3f9851c9-2c52-4818-a767-30f5c6be361c)
* ![image](https://github.com/jude-lindale/SEC-480/assets/70959569/c149b4eb-8cac-4041-921a-ef35dfd8da8f)



* Vulnerability Detection
* ![image](https://github.com/jude-lindale/SEC-480/assets/70959569/1efcc2cd-94a4-49b3-b5ff-ab7c9407119f)
* ![image](https://github.com/jude-lindale/SEC-480/assets/70959569/0125c194-e6ca-433f-85a4-06d52a519b35)
![image](https://github.com/jude-lindale/SEC-480/assets/70959569/1a69bea2-96fc-4193-98f8-5fa2c290c7e3)
![image](https://github.com/jude-lindale/SEC-480/assets/70959569/9a5fae2f-cc4b-4121-99e2-cd184f6557af)
![image](https://github.com/jude-lindale/SEC-480/assets/70959569/4dfff5e3-9cc2-45c8-9f65-104555b165ec)
![image](https://github.com/jude-lindale/SEC-480/assets/70959569/fd98b9fb-814c-4824-aaa3-f8b3247ea146)
![image](https://github.com/jude-lindale/SEC-480/assets/70959569/7cc3a9af-51ed-4b7a-9246-d2cef1bbe712)



* Foothold
* Privilege Escalation
* Proof
* Post Exploitation (Loot)
