
### Executive Summary

On March 07, 2024 at 11:44 AM, a remote desktop brute force attempt alert was generated. This reason for this trigger was the IP address 218.92.0.56 had multiple attempts to log on to the network with different credentials, indicating highly suspicious behavior. Upon further investigation, it was identified that this IP was able to access the user account of Mathew with the remote desktop program "tvnserver.exe." After access to the network, the attacker ran commands to gather details on the user account, the administrator accounts, and the active network connections. Since identification of the unauthorized access, the device has been disconnected from the network and the IP address has been blocked.
### Alert Details
- Alert Name: SOC176 - RDP Brute Force Detected
- EventID: 234
- Event Type: Brute Force
- Date: Mar 07 2024 - 11:44 AM
- Source IP: 218.92.0.56
- Destination IP: 172.16.17.148 - Mathew
- Trigger Logic: Login failure from single source with different non-existing accounts
- Initial Classification: Suspicious
### Investigation
1. Created a case as the alert seemed to be suspicious
	- The alert trigger seems like it warrants further investigation as the behavior is abnormal.
2. Checked the IP address against VirusTotal for initial confirmation.
	- 9/94 vendors on VirusTotal indicate that the IP address is malicious, but it cannot completely confirmed without more investigation.
3. Pivoted to the endpoint for further analysis
	- Looked at the terminal history first and found that one minute after this alert generated, there was a whoami command to identify the user "letsdefend." 
	- Then the net user letsdefend command was used to gather information on the user, net localgroup administrators was run right after, gathering information on the admins. 
	- Then netstat -ano was run, a command used to display the active network connections and their process ID.
	- From just the terminal, it seems very likely that an attacker is on the device.
	- I then moved onto the processes run, and identified a suspicious command.
		- "C:\Program Files\TightVNC\tvnserver.exe" -desktopserver -logdir "C:\Windows\system32\config\systemprofile\AppData\Roaming\TightVNC" -loglevel 0 -shmemname Global\heiejorlvibfsxlitbor
	- The program tvnserver.exe is a remote desktop tool and could be highly indicative of a malicious remote desktop session. 
	- As all these seemed like indicators of compromise, I decided to contain the system by temporarily disabling it's connection to the network.
4. I then wanted to gather more information and moved over to the SIEM to analyze its logs by querying for the source address: 218.92.0.56.
	- The first log was at 11:44 AM and shows the username 'admin' with 4625 event ID, which is an unsuccessful logon with incorrect credentials.
	- There is a second event ID 4625 but with the guest username instead.
	- After these attempts, I am able to see multiple attempts to connect to 172.16.17.148 through port 3389, which is RDP.
	- I then find a log with the EventID 4624, 'a successful logon' over port 3389, which is indicative of the unauthorized remote desktop session.
### Findings

This alert was a successful remote desktop attack with the tvnserver program over port 3389 with a brute force approach. The IOAs were the the multiple attempts to connect to the server with various different usernames, but the same IP address. 
The IOCs are the Event ID 4624 to the user 'Mathew', the whoami and netstat commands, and the execution of the tvnserver.exe program. The user Mathew has been access by an unauthorized individual who has information the administrators on the system. Additionally, it is unknown if there was any lateral movement.

MITRE ATT&CK TTPs
- T1110.001 - Brute Force: Password Guessing
- T1021.001 — Remote Services: Remote Desktop Protocol
- T1033 — System Owner/User Discovery (whoami, net user)
- T1069.001 — Permission Groups Discovery (net localgroup administrators)
- T1049 — System Network Connections Discovery (netstat -ano)
### Recommendations

1. Disconnect the device from the network and the internet.
2. Block access to the IP address 218.92.0.56.
3. Perform further analysis to identify if lateral movement has occurred.
4. Update all administrator account credentials.
5. Update Mathew's account credentials.
6. Enable MFA on all user accounts.
### Lessons Learned

It is important to maintain strong passwords, however, they can are still vulnerable as they do not provide defense in depth. To add a layer of security, it is imperative that Multi-Factor authentication is implemented, so that in the case that a brute force attack successfully detects a user's password, the attacker will require further authentication.
