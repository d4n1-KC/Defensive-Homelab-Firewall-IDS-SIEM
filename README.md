# Blue Team Home Lab – Firewall, IDS & SIEM

This repository documents a hands-on **Blue Team homelab** designed to simulate a small corporate network with a strong focus on **defensive security, monitoring, and incident detection**.

The lab was built in a fully controlled environment and aims to demonstrate how different defensive components interact in a real-world–like infrastructure: **network segmentation, firewall enforcement, intrusion detection, log centralization, and event correlation**.

---

## Lab Objectives

- Design and deploy a segmented network following **defense-in-depth** principles.
- Enforce strict traffic control using a **stateful firewall**.
- Detect suspicious and malicious activity through an **IDS**.
- Centralize logs and alerts in a **SIEM** for correlation and analysis.
- Gain hands-on experience in **Blue Team operations** and security monitoring workflows.

---

## Network Architecture

The environment is built around a **pfSense firewall/router** acting as the central control point between the WAN and internal networks.

### Network Segments

- **LAN (192.168.100.0/24)**  
  Internal workstation simulating a corporate endpoint (Windows).  
  Used to observe endpoint activity and internal traffic.

- **DMZ (192.168.200.0/24)**  
  Honeypot system intentionally exposed to external access.  
  Designed to capture attacker behavior and executed commands.

- **DMZ2 (192.168.250.0/24)**  
  Linux server hosting an Apache web service and **Suricata IDS**.  
  Acts as the monitored public-facing service.

---

## Firewall & Traffic Control (pfSense)

The firewall configuration enforces:

- Strict **network isolation** between LAN, DMZ and DMZ2.
- Controlled **NAT rules** to expose only required services:
  - HTTP (80) → Web server (DMZ2)
  - SSH (22 → 222) → Honeypot (DMZ)
- Least-privilege access between subnets.
- Clear rule hierarchy to avoid unintended traffic leaks.

This setup ensures that internal assets remain protected while exposed services can be monitored safely.

---

## Intrusion Detection – Suricata

Suricata is deployed in **IDS (passive) mode** on the DMZ2 server.

Custom detection rules were created to identify:

- HTTP traffic directed to the web server.
- Authentication attempts on the application.
- Brute-force or credential stuffing patterns.
- Abnormal request rates indicating fuzzing or scanning activity.

Alerts are generated in **EVE JSON format**, allowing seamless integration with the SIEM.

---

## SIEM & Log Correlation – Elastic Stack

All hosts run an **Elastic Agent**, forwarding logs to Elasticsearch.

Custom agent policies were created for:

- **Suricata alerts**
- **Windows security and PowerShell activity**
- **Honeypot command execution logs**

Using **Kibana**, logs are analyzed through:
- Dedicated data views per asset.
- Filters based on signatures, source IPs and timestamps.
- Dashboards showing alert volume, top IPs and event trends.

This enables effective **event correlation and early detection of attack patterns** across the environment.

---

## Key Takeaways

Through this lab I gained practical experience in:

- Firewall rule design and traffic segmentation.
- IDS rule creation and alert tuning.
- SIEM-based log analysis and correlation.
- Observing attacker behavior via honeypots.
- Understanding how defensive controls work together in a modern infrastructure.

---

## Disclaimer

All systems, IP addresses, data and events in this lab are **entirely fictional** and deployed in isolated environments for educational purposes only.
