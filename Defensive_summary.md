# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology
![alt-txt](https://github.com/sparkplug007/Final-Project/blob/main/images/blue_file/diagram.io.png)

The following machines were identified on the network:
- Kali machine
  - **OS**         : Linux 2.6.32
  - **Purpose**    : Attack machine
  - **IP Address** : 192.168.1.90
- ELK
  - **OS**         : Linux/Ubuntu 18.04
  - **Purpose**    : ELK stack
  - **IP Address** : 192.168.1.105
- Capstone
  - **OS:**         Linux/Ubuntu 2.4.29
  - **Purpose:**    Apache server
  - **IP Address:** 192.168.1.105
- Target 1 machine
  - **OS:**         Linux/Debian 3.2-4.9
  - **Purpose:**    Apache server/ Vulnerable WordPress server
  - **IP Address:** 192.168.1.110
- Target 2 machine
  - **OS:**         Linux 3.2
  - **Purpose:**    Web server
  - **IP Address:** 192.168.1.115
  

### Description of Targets

The target of this attack was: `Target 1` (192.168.1.110).

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### HTTP Request Size Monitor
HTTP Request Size Monitor was implemented as follows:
  - **Metric**: WHEN sum () of `http.request.bytes` OVER all documents
  - **Threshold**: IS ABOVE 3500 FOR THE LAST 1 minute
  - **Vulnerability Mitigated**: Measure the volume of traffic that might indicate an attack
  - **Reliability**: Highly reliable when CPU resources is compromised and slowing responses within local machines.
  ![alt-txt](https://github.com/sparkplug007/Final-Project/blob/main/images/blue_file/HTTP%20request%20size%20monitor1.png)
  
#### Excessive HTTP errors
Excessive HTTP errors is implemented as follows:
  - **Metric**: WHEN count () GROUPED OVER top 5 `http.response.status_code` 
  - **Threshold**: IS ABOVE 400 FOR THE LAST 5 mins
  - **Vulnerability Mitigated**: Brute Force Attack, resources usage issues
  - **Reliability**: High reliability. Aletrs would trigger for excessive failed logins or bad request if server have advanced security setup.
  ![alt-txt](https://github.com/sparkplug007/Final-Project/blob/main/images/blue_file/Excessive%20HTTP%20errors1.png)

#### CPU Usage Monitor
CPU Usage Monitor is implemented as follows:
  - **Metric**: WHEN max () OF `system.process.cpu.total.pct` OVER all documents
  - **Threshold**: IS ABOVE 0.5 FOR THE LAST 5 minutes
  - **Vulnerability Mitigated**: Resources management, excessive CPU usage
  - **Reliability**: Medium. It depends on the application that is running that might generate a false positives.

  ![alt-txt](https://github.com/sparkplug007/Final-Project/blob/main/images/blue_file/CPU%20Usage%20Monitor1.png)
 
_TODO Note: Explain at least 3 alerts. Add more if time allows._

### Suggestions for Going Further (Optional) 
- Each alert above pertains to a specific vulnerability/exploit. Recall that alerts only detect malicious behavior, but do not stop it. For each vulnerability/exploit identified by the alerts above, suggest a patch. E.g., implementing a blocklist is an effective tactic against brute-force attacks.

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:
- Vulnerability 1 : Weak configuration of password leading to SSH protocol exploit
  - **Patch**: Modify default ports to custom port. Monitor unauthorized access to this port. Allow ONLY specific known Clients to connect.
  - **Why It Works**: By default, SSH is set to be listening on port 22. Changing the default port 22 to a different port will enhance additional layer of security in your network.
- Vulnerability 2 : Enumeration attack - WordPress server
  - **Patch**: Disable the WordPress REST API, Disable WordPress XML-RPC if you are not using it, Configure your web server to block requests to /?author=<number>, and don’t expose /wp-admin and /wp-login.php directly to the public Internet
  - **Why It Works**: WordPress user enumeration works on every WordPress site by default because of a WordPress feature called permalinks. Permalinks are permanent URLs to individual WordPress posts and pages.In addition to post and pages, it allows to list all posts by a particular author's username.
- Vulnerability 3 : Weak WordPress /wp-config.php security implementation
  - **Patch**: Back-up regularly WordPress server, enhance security by updating CMS, Plugins & Themes to the latest Versions, update *.php to the latest version, remove defunct plugins/themes.
  - **Why It Works**: The wp-config.php is an important file for every WP installation. It is the configuration file used by the site and acts as the bridge between the WP file system and the database. The wp-config.php file contains sensitive information such as: Database host, database name, username, password and port numbers. Once hackers get hold of the database login details via the wp-config.php, they try to connect to the databse and create fake WordPress accounts or target an existing user account for priviledge escalation and traversing into the compromised database.
  
### References:
  
  - a.https://www.getastra.com/blog/911/wordpress-files-hacked-wp-config-php-hack/
  - b.https://www.getastra.com/blog/cms/wordpress-security/wordpress-security-guide/
  - c.https://www.wpwhitesecurity.com/enumerate-wordpress-users-wpscan-security-scanner/
  - d.https://hackertarget.com/attacking-wordpress/
