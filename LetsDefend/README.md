This document outlines my personal triage procedure for alert investigations conducted in environments with minimal automation, specifically those lacking automated threat enrichment or lack of built-in MITRE ATT&CK mapping.
### Procedure
1. Review the alert trigger to understand what detection rule was fulfilled and form an initial hypothesis about the attack type.
2. Document all available alert metadata: source IP, destination IP, affected host, timestamps, and any file hashes or URLs present in the alert.
3. Run any public IP addresses or file hashes through a threat intelligence platform (VirusTotal, AbuseIPDB) to establish an early confidence level on maliciousness before exploring logs.
4. Pivot to the affected endpoint and review terminal history, running processes, and browser history. Filter initially to a 15-minute window prior to the alert and expand it if activity of interest extends further back.
    - For any unfamiliar processes or commands, research or use GenAI to understand the intended behavior before drawing conclusions.
    - If malicious behavior is confirmed on the endpoint, disconnect it from the network immediately before continuing the investigation.
5. Move to SIEM log management to map the attacker's initial access. Correlate the alert event with surrounding log activity. F
	- For example, a suspicious successful logon is often preceded by failed logon attempts from the same source IP that reveal the brute force stage.
6. Document attacker intent at each stage as the investigation progresses, even if the hypothesis changes. Correcting a wrong hypothesis is part of the analytical record and should not be excluded.
7. Summarize findings with the initial access vector, actions taken on objective, affected accounts and systems, and map all identified behaviors to MITRE ATT&CK TTPs.
8. Produce recommendations covering immediate containment, credential and system remediation, and any detection rule tuning or additions identified during the investigation.
9. Document lessons learned specific to this incident, what the investigation revealed about the environment's security posture and why each recommendation is relevant.
10. Write the executive summary last starting with, the timeline of the attacker's actions with specific timestamps, briefly describe the investigative process followed, and state what mitigation actions have already been taken.
