# SecOps Architecture from Scratch (AiTM & IR Automation)

### Project Vision
This project is a high-fidelity simulation of a modern enterprise security environment. It demonstrates an end-to-end Adversary-in-the-Middle (AiTM) attack chain and a corresponding, automated Security Operations (SecOps) pipeline for detection and case management.

The goal is to prove that traditional MFA is insufficient against proxy-based attacks and to showcase how automation (SOAR) can bridge the gap between detection and incident response.

---

### System Architecture
The entire ecosystem is containerized using Docker, ensuring a portable and isolated lab environment.

#### Defensive Stack (The SOC)
* ELK Stack (Elasticsearch, Logstash, Kibana): Centralized SIEM for log aggregation, analysis, and security visualization.
* n8n: The automation engine acting as a lightweight SOAR platform. It parses attack telemetry and orchestrates the alerting flow.
* DFIR IRIS: A professional-grade Case Management System used to track incidents and manage evidence.

#### Offensive Stack (The Adversary)
* Evilginx2: Advanced man-in-the-middle attack framework used for transparent proxying and session hijacking (MFA Bypass).
* WebStop: A dedicated victim container used to simulate realistic user behavior and endpoint telemetry.

---

### MITRE ATT&CK Mapping

| Tactics | ID | Technique | Description |
| :--- | :--- | :--- | :--- |
| Reconnaissance | T1589 | Gather Victim Identity Information | Identification of targets via OSINT (LinkedIn/Employee lists). |
| Resource Development | T1583.001 | Domestic Domains | Orchestrating typosquatted infrastructure (e.g., egyetem1.com). |
| Initial Access | T1566.002 | Spearphishing Link | Delivery of the malicious proxy-link to the target. |
| Credential Access | T1557.002 | Adversary-in-the-Middle | Real-time interception of credentials and session tokens. |

---

### Automated Workflow (The Pipeline)
1. Exploitation: Evilginx captures a successful login and exports the session cookie.
2. Log Shipping: Raw attack logs are ingested by the n8n workflow.
3. Parsing & Enrichment: n8n extracts key IoCs (IP addresses, User-Agents, Timestamps).
4. Alerting: n8n triggers a webhook to DFIR IRIS, automatically creating a new Case for the SOC team.
5. Visualization: Kibana dashboards display the frequency and origin of the AiTM traffic for real-time monitoring.

---

### Technologies Used
* Virtualization: Docker / Docker-Compose
* SIEM: Elasticsearch 8.x
* Automation: n8n (Workflow-based)
* Case Management: DFIR IRIS
* Attack Framework: Evilginx2
