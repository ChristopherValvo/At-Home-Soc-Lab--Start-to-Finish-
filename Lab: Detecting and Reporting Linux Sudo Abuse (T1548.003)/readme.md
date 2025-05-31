This Splunk lab simulates a real SOC detection pipeline — from log ingestion to alerting to report generation — using Linux audit logs to catch unauthorized sudo activity.

Skills Practiced
Splunk Data Ingestion

MITRE ATT&CK T1548.003 Detection

Auditd Log Analysis

Splunk Dashboard & Alerting

SOC Playbook Development

Incident Report Writing

Want to Try This Lab Yourself?
1. Enable audit logs on Kali Linux:

bash
Copy
Edit
sudo apt update && sudo apt install auditd -y
sudo systemctl enable auditd --now
2. Configure Splunk to monitor /var/log/audit/audit.log:

In Splunk Web:
Settings > Add Data > Monitor > Files & Directories
Path: /var/log/audit/audit.log
Sourcetype: linux_audit
Index: linux_audit

3. Run this to simulate malicious activity:

bash
Copy
Edit
sudo cat /etc/shadow
4. SPL to detect sudo misuse:

spl
Copy
Edit
index=linux_audit sourcetype=linux_audit
| regex _raw=".*sudo.*"
| table _time host user exe msg
5. Save the query as an alert:

Trigger when results > 0
Title: Alert – Suspicious Sudo Activity (T1548.003)
Schedule: Every 5 min

6. Build a dashboard:

"Top Sudo Commands by User" → bar chart

"Sudo Events Over Time" → line chart

"Raw Sudo Event Log" → table

7. Document your response plan:

Sample Playbook Entry
Playbook Step: Detect Unauthorized Sudo Execution
Technique: T1548.003 – Abuse Elevation Control Mechanism
Alert Name: Alert – Suspicious Sudo Activity
SPL:

spl
Copy
Edit
index=linux_audit sourcetype=linux_audit
| regex _raw=".*sudo.*"
| stats count by user, exe
Checklist:

Confirm user context (admin or attacker?)

Review command intent

Correlate with other data sources

Response Actions:

Notify analyst

Escalate to IR

Quarantine host if confirmed malicious

![Dashboard](https://github.com/user-attachments/assets/53433ffc-174e-4276-8fe6-351a84f9477d)
![Incident report part 1](https://github.com/user-attachments/assets/e0dd7cba-306e-4d55-8fd4-e690800743d9)
![Incident report part 2](https://github.com/user-attachments/assets/86d9d53c-37f8-400f-9ac3-52a20ccc2e39)


#cybersecurity
#splunk
#socanalyst
#linuxsecurity
#threatdetection
#infosec
#mitreattack
#incidentresponse
#loganalysis
#securityoperations
