# SOC Investigation Portfolio

Documented alert triage and incident investigations across multiple SOC environments and platforms. Each writeup follows a structured incident report format covering alert details, investigation process, MITRE ATT&CK mapping, findings, and recommendations.

---

## Repository Structure

```
soc-investigations/
├── letsdefend/           # Investigations from LetsDefend's simulated SOC platform
└── elk-stack/            # Investigations from a self-hosted ELK Stack SOC lab (coming soon)
```

---

## Environments

### LetsDefend
Blue team training platform with a simulated SOC environment including a live alert queue, SIEM log management, endpoint analysis, threat intelligence lookups, and case management. Investigations cover real-world attack scenarios across web attacks, brute force, malware, phishing, and more.

### ELK Stack Lab *(coming soon)*
Self-hosted Elasticsearch, Logstash, and Kibana environment built from scratch. Investigations will cover custom detection rule development, log ingestion pipeline tuning, and threat hunting alongside alert triage — bridging analyst work with detection engineering.

---

## Investigation Index

### LetsDefend

| # | Alert | Technique | MITRE ATT&CK | Attack Outcome |
|---|-------|-----------|--------------|---------|
| 1 | SOC176 - RDP Brute Force Detected | Brute Force + Remote Desktop | T1110.001, T1021.001 | Successful |
| 2 | SOC165 - Possible SQL Injection Payload Detected | Exploitation of Public-Facing Application | T1190 | Unsuccessful |
| 3 | SOC335 - CVE-2024-49138 Exploitation Detected | Brute Force + Remote Desktop + Privlige Escalation | T1110.001, T1021.001 | Successful |

### ELK Stack

| # | Investigation | Technique | MITRE ATT&CK | Outcome |
|---|---------------|-----------|--------------|---------|
| — | — | — | — | — |

---

## Writeup Format

Each investigation follows this structure:

- **Executive Summary** — Non-technical overview of the incident and outcome
- **Alert Details** — Raw alert metadata from the SIEM queue
- **Investigation Process** — Step-by-step analytical narrative with pivots and reasoning
- **Findings** — Verdict, MITRE ATT&CK mapping, IOCs, affected assets, and timeline
- **Root Cause** — Initial access vector or trigger
- **Recommendations** — Immediate containment, remediation, and detection improvement
- **Lessons Learned** — Investigative takeaways and process improvements

---

## Skills Demonstrated

- Alert triage and prioritization
- SIEM log analysis and querying (Kibana, LetsDefend SIEM)
- Endpoint investigation (process trees, terminal history, network connections)
- MITRE ATT&CK technique identification and mapping
- IOC and IOA documentation
- Incident containment decision-making
- Detection gap analysis and rule improvement recommendations

---

*Part of a broader security portfolio — see other repositories for SOAR pipeline engineering, Microsoft Sentinel KQL detection rules, and cloud security projects.*
