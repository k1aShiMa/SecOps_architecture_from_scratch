# ChangeLogs

### Development Status & Road Map: 2026.03.10

**Achievements**: So today I've managed to run DFIR-IRIS. I've made an update for this, because i couldn't push everything into one docker-compose file.

**Technical Challenge**: Currently troubleshooting inter-container networking. I am refining the Docker bridge network configuration to ensure seamless communication between the SIEM and the Attack nodes.

---

### Deployment Notes: 2026.03.12

**DFIR-IRIS**: The DFIR-IRIS instance is intentionally decoupled from the main docker-compose file to maintain modularity and manage resource allocation more effectively. To integrate it, follow the official IRIS deployment guide and link it to the n8n webhook via its API.

**Where to find IRIS?**: https://github.com/dfir-iris/iris-web --> Here you can find the repo, you'll find everything in here for alert & case management.

**Final Note on Deployment & Scaling**: This stack is provided as a functional blueprint. To make it operational, you'll need to tailor the configurations to your specific environment — if you don't tune it, it won't work. While this "Cheap-Ops" architecture is designed to scale into a full-blown enterprise SOC, doing so requires significant resources that exceed my current 7.5GB RAM lab constraints. It's built to be big, just give it the iron to run on.

**Another Note for n8n**: The automation flows (playbooks) are left for you to build. Instead of providing pre-baked JSONs, I’ve laid out the directory structure for the whole operation. Build your own logic, trigger your own alerts the infrastructure is ready, the automation brain is up to you.

---

### Deployment Notes: 2026.03.26

**Packetbeat**: The stack now integrates Packetbeat to provide deep visibility into network-level telemetry, addressing the inherent limitations of file-based monitoring (Filebeat) for real-time traffic analysis. While Auditbeat was initially evaluated for endpoint auditing, the current Windows-based host environment presented permission constraints that hindered its effectiveness. Consequently, Packetbeat was selected as the primary engine for wire-protocol analysis.

* **Next Phase**: Engineering custom detection rules within the SIEM to validate the end-to-end telemetry pipeline and stress-test the detection logic.

**SOAR & Orchestration Strategy**: Development is shifting toward the SOAR layer and automated data pipelines. To maintain the "Build-Your-Own-Logic" philosophy of this lab, raw n8n JSON exports will not be provided. Instead, the repository will be updated with high-resolution architectural screenshots of the workflows. This approach ensures that you understand the operational flow and dependencies before manually configuring the automation to suit your specific environment.

---

### 99.6% Pure Telemetry

* No half measures. The architecture is solid, the telemetry is flowing. Now it's time to watch the blue smoke.