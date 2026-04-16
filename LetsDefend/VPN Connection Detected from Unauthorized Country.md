### Executive Summary

On February 13, 2024 at 2:04 AM, the SIEM recognized a connection attempt from an unauthorized country. Upon further investigation, at 1:54 AM, there was connection from Hanoi, Vietnam to the 172.16.17.163 (account belonging to Monica) with the correct credentials. However, the attacker was not able to access the system as multi-factor authentication was enabled and the attacker could not access the OTP sent to Monica's email. Monica's password has been updated since and she has been informed on limiting password reuse across external accounts. Additionally, all incoming connections from unauthorized countries will be blocked to address the 11 minute window between the access attempt and alert generation.
### Alert Details
- Alert Name: SOC257 - VPN Connection Detected from Unauthorized Country
- EventID: 225
- Event Type: Unauthorized Access
- Date: Feb 13 2024 - 2:04 AM
- Source IP: 113.161.158.12
- Destination IP: 172.16.17.163 - Monica
- Trigger Logic: VPN Connection Detected from Unauthorized Country
- Initial Classification: Suspicious
### Investigation
1. Checked to see where the IP originated from.
	- It originated from Vietnam, so I checked to see if Monica or any users in our network were currently in Vietnam (this is a simulation so it's not possible to check, but it's what I would do in this situation)
2. Ran the IP address in VirusTotal.
	- 8/94 vendors state that it is malicious, so I should proceed as if it is a threat actor.
3. Pivoted to the 172.16.17.163 endpoint for further investigation.
	- Browser history looks normal
	- Terminal history shows information gathering commands from Feb 13 8:00 AM to 10:45 AM, such as ipconfig, systeminfo, netstat -ano-, net user, etc. This looks highly indicative of attacker behavior.
	- No other information from processes.
	- I chose to disconnect the system as I believe an attacker has attempted to gather information about this endpoint.
4. Pivoted to SIEM logs to map the attacker timeline.
	- At 1:54 AM, the source ip '113.161.158.12' started multiple attempts to connect to the VPN at the address '33.33.33.33', the logs were collected by the firewall.
	- At 2:02 AM, the was able to connect to 172.16.17.163 through 33.33.33.33:443 (HTTPS) but was stopped after inputting the wrong one-time password code.
	- There was a POST request with a status code 200, indicating that the attacker has the right credentials.
		- Date=13/Feb/2024:02:02:13+0000, URL=https://vpn-letsdefend.io, Source IP=113.161.158.12, Request=POST, URI=logon.html, Protocol=HTTP/1.0, Response Status=200, Username=Monica@letsdefend.io
	- So far, it does not seem like the attacker was able to access the 172.16.17.163 endpoint.
5. Pivoted to the Email logs to check the OTP email sent to Monica at 172.16.17.163
	- There were three emails sent to Monica with the OTP, at 2:01, 2:02, and 2:03 AM about an IP from Hanoi, Vietnam trying to access the endpoint.
### Findings

The alert indicated an attempt to access the internal network from an unauthorized country. The attacker had the correct credentials to the user 'Monica' but could not get past the MFA, an email with an OTP sent to Monica's email. This is a good example of why defense in depth is essential in networks. Additionally, there was an 11 minute difference between connection time and alert generation, this needs to be minimized.

MITRE ATT&CK TTPs
- T1078.003 - Local Accounts, Using compromised local user credentials
- T1621 - MFA Authentication Request Generated
### Recommendations
1. Update Monica's credentials.
2. Investigate further on how Monica's credentials got leaked.
3. Add a section on password reuse in the next security awareness training.
4. Block incoming connection attempts from unauthorized countries.
### Lessons Learned

This case is a solid example of why Multi-Factor Authentication should be enabled on all accounts on a network. Without the MFA, the attacker would have been able to access the network with the stolen credentials. It is also imperative that users maintain strong passwords and limit reusing passwords across external accounts. 

Looking back at the the disconnection of the endpoint from the network, it was premature based on the rest of investigation and I should establish a stronger baseline before containment. However, the commands are slightly unusual and I should talk with Monica on whether or not she performed those.
