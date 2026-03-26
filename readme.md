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

### Design Philosophy: Why This Stack?

The selection of these specific technologies wasn't accidental, it was driven by a need for high performance on a "shoestring" budget. Essentially only 7.5GB of RAM is available

* **Logstash vs. Filebeat**: Logstash was dropped from the final architecture because it is a notorious RAM-hog. In a containerized lab, efficiency is king. **Filebeat** is the superior choice here, it’s incredibly lightweight, fast, and does the job with a fraction of the footprint. It's the "geek's choice" for shipping logs without melting your CPU.
* **Packetbeat**: I couldn't find any better idea to monitor the traffic in this project.
* **The "Cheap-Ops" Advantage**: This entire SecOps pipeline is built using open-source or community-tier tools. It proves that you don't need a six-figure licensing deal with a legacy vendor to build a world-class detection engine.
* **Scalability on Demand**: Because everything is containerized and uses modular tools like n8n and Elasticsearch, the architecture scales horizontally. Whether you are monitoring one container or a hundred, the pipeline holds up without breaking the bank.

---

### Technologies Used
* **SIEM**: Elasticsearch (The Brain)
* **Shipper**: Filebeat & Packetbeat (The lightweight goats)
* **SOAR**: n8n (Orchestrating the pain)
* **Case Management**: DFIR-IRIS (Backbone of the alert & case management)
* **Attack**: Evilginx2 (MFA? What MFA?)
* **Host Machine**: Webtop (Simulating the victim environment)
* **Virtualization**: Docker / Docker-Compose (Orchestrating the lab)

---

### Development Status & Road Map: 2026.03.10

**Achievements**: So today I've managed to run DFIR-IRIS. I'll make an update for this, because i couldn't push everything into one docker-compose file.

**Technical Challenge**: Currently troubleshooting inter-container networking. I am refining the Docker bridge network configuration to ensure seamless communication between the SIEM and the Attack nodes.

---

### Deployment Notes: 2026.03.12

**DFIR-IRIS**: The DFIR-IRIS instance is intentionally decoupled from the main docker-compose file to maintain modularity and manage resource allocation more effectively. To integrate it, follow the official IRIS deployment guide and link it to the n8n webhook via its API.

**Where to find IRIS?**: https://github.com/dfir-iris/iris-web - Here you can find the repo, you'll find everything in here for alert & case management.

**Final Note on Deployment & Scaling**: This stack is provided as a functional blueprint. To make it operational, you'll need to tailor the configurations to your specific environment — if you don't tune it, it won't work. While this "Cheap-Ops" architecture is designed to scale into a full-blown enterprise SOC, doing so requires significant resources that exceed my current 7.5GB RAM lab constraints. It's built to be big, just give it the iron to run on.

**Another Note for n8n**: The automation flows (playbooks) are left for you to build. Instead of providing pre-baked JSONs, I’ve laid out the directory structure for the whole operation. Build your own logic, trigger your own alerts the infrastructure is ready, the automation brain is up to you.

---

### Deployment Notes: 2026.03.26

**Packetbeat**: The stack now integrates Packetbeat to provide deep visibility into network-level telemetry, addressing the inherent limitations of file-based monitoring (Filebeat) for real-time traffic analysis. While Auditbeat was initially evaluated for endpoint auditing, the current Windows-based host environment presented permission constraints that hindered its effectiveness. Consequently, Packetbeat was selected as the primary engine for wire-protocol analysis.

* **Next Phase**: Engineering custom detection rules within the SIEM to validate the end-to-end telemetry pipeline and stress-test the detection logic.

**SOAR & Orchestration Strategy**: Development is shifting toward the SOAR layer and automated data pipelines. To maintain the "Build-Your-Own-Logic" philosophy of this lab, raw n8n JSON exports will not be provided. Instead, the repository will be updated with high-resolution architectural screenshots of the workflows. This approach ensures that you understand the operational flow and dependencies before manually configuring the automation to suit your specific environment.

---

### Cook Your Own Blue

* The lab is set. The precursors are in the tanks. Now it’s your turn to cook the logic. This is a blueprint: the infrastructure is ready, but the final automated response is yours to craft. Stay out of my territory.