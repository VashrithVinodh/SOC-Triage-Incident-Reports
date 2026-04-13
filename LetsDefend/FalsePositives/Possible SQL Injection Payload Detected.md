
### Executive Summary

On Febuary 25, 2022 at 11:34 AM, an external threat actor at 167.99.169.17 attempted to access our database on WebServer1001 with and SQL injection attack. The attack failed and our server returned HTTP 500, an internal error, indicating our server did not return the database. No data was exfiltrated and no accounts were compromised. The IP address has been blocked.

### Alert Details

Alert Name: SOC165 - Possible SQL Injection Payload detected <br>
EventID: 115 <br>
Event Type: Web Attack <br>
Date: Feb 25 2022 - 11:34 AM <br>
Source IP: 167.99.169.17 <br>
Destination IP: 172.16.17.18 - webserver1001 <br>
Trigger Logic: Requested URL contained "OR 1=1" <br>
Initial Classification: Suspicious

<img width="1581" height="700" alt="image" src="https://github.com/user-attachments/assets/c08c7c01-9841-44f1-8281-2fa59df4e8dc" />

### Investigation Process

1. Created a case as the alert seemed suspicious
	- The alert trigger reason has led to this suspicious as it seems like an SQL injection attack against one of our internal servers. Especially due to a public source address initiating a connection our local destination address, which holds our server "WebServer1001".
2. Located the endpoint device to investigate further
	- Found that this server's primary user is the webadmin, so access to this server could potentially mean admin privileges.
	- Was not able to find anything else here, there were processes or commands run after the time this alert was generated.
3. Changed over to the log management to map behavior after the potential injection attack
	- Queried for the source address "167.99.169.17" to find all instances of the user attempting to access our network.
	- The SIEM logs showed that the SQL injection attempt failed as the server returned a response code of 500.
	- The same IP address also tried other another injection attack with OR 'x' = 'x' that also failed and return the status code 500.

### Findings

This alert was a failed SQL injection attack with a MITRE ATT&CK technique of T1190. The indicators of attack were the attempts to access database with OR 1=1, OR 'x' = 'x'. The web server follows proper input validation and returned an internal error code of 500. There were no users or accounts attacked or compromised.

### Recommendations 

1. Ensure that the IP address is blocked
2. Verify that other web servers have proper input validation measures in place

### Lessons Learned

It is important to ensure that there is proper input validation with public facing web servers. Attackers will try to attack these points as they provide an entry into our network.
