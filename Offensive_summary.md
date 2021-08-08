# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

Nmap scan results for each machine reveal the below services and OS details:

```bash
$ nmap -A 192.168.1.0/24
  # TODO: Insert scan output
```

This scan identifies the services below as potential points of entry:
- Target 1 ( IP 192.168.1.110 )
  - port 22/tcp   open   ssh
  - port 80/tcp   open   http
  - port 111/tcp  open   rpcbind
  - port 139/tcp  open   netbios-ssn
  - port 445/tcp  open   microsoft-ds

_TODO: Fill out the list below. Include severity, and CVE numbers, if possible._

The following vulnerabilities was identified on target machine:
- Target 1
  - Default port SSH service
  - Sniff and Capture credentials over a non-secure credentials
  - Unsalted hash on passwords
  - WordPress enumeration
  - Compromised Systems admin tools 
  - WordPress xmlrpc.php attack ( CVE-1999-0502)
  - WordPress Core ( CVE-2019-8943)

_TODO: Include vulnerability scan results to prove the identified vulnerabilities._

### Exploitation
_TODO: Fill out the details below. Include screenshots where possible._

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: b9bbcb33e11b80be759c4e844862482d
    - **Exploit Used**
      - _Enumeration attack on users using WPscan on target machine
  ```bash
  $ wpscan --url http://192.168.1.110/wordpress --enumerate u  
      - _Being able to identify users, the next step is to be able to identify the "password"
         The most obvious one it to guess the password or conduct a hydra brute force attack
      - _By guessing the password for user -michael as password="michael" I was able to connect
         to the target machine by using open port 22 through ssh protocol
      - _Traversing through the folders `$ cd /var/www/html` found file _service.html
         
  - `flag2.txt`: _`fc3fd58dcdad9ab23faca6e9a36e581c` 
    - **Exploit Used**
      - _Enumeration attack on users using WPscan ( same as above)
      - _After having control over the user "michael's" account I was able to find:
```bash
  $ find / -name flag*.txt
_insert png
      - _Identify a folder that has "flag2.txt" on it
```bash
  $ cd /var/html
_insert png
