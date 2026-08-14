# Indicators of Compromise (IOC) Report
## Network Traffic Analysis for Threat Detection

---

## IOC 1: TCP Port Scanning Activity (SYN Scan)

- **Indicator Type:** Port Scanning (Reconnaissance)
- **Detection Tool:** Wireshark (packet capture), Nmap (scan source)
- **Source:** 127.0.0.1 / ::1 (localhost, loopback interface)
- **Destination:** 127.0.0.1 / ::1 — multiple destination ports: 5432, 6432, 51012, 5435, 8089, 135, 139, 445, 7070, 8000
- **Protocol:** TCP
- **Flags Observed:** SYN flag set, ACK flag not set (connection initiation only)
- **Pattern Observed:** A single source generated multiple SYN packets to many different destination ports within a very short time window (sub-second to few-second intervals). Each probe used a new/incrementing source port.
- **Response Pattern:** Ports with active services responded with SYN-ACK (e.g., 5432, 6432, 51012, 5435, 8089); closed ports did not respond or reset the connection.
- **Tool Signature:** Confirmed via Nmap SYN Stealth Scan (-sS) output showing identical port list (135, 139, 445, 5432, 7070, 8000, 8089) as "open."
- **Significance:** Classic signature of a TCP SYN (stealth) port scan, commonly used during the reconnaissance phase of an attack to enumerate open ports and running services on a target host.
- **Recommended Action:** Monitor for repeated SYN packets to multiple ports from a single source in production environments; configure firewall/IDS rules to alert on scan-like behavior.

---

## IOC 2: High-Volume Data Transfer to Single External Host

- **Indicator Type:** Large Data Transfer / Top Talker
- **Detection Tool:** Wireshark (Statistics > Conversations)
- **Source:** 192.168.0.110 (local host)
- **Destination:** 202.88.185.201 (external IP)
- **Protocol:** TCP/UDP (mixed)
- **Pattern Observed:** 7,311 packets and approximately 8 MB of data exchanged with a single external IP address, significantly higher than any other conversation in the capture.
- **Significance:** In this case, identified as legitimate video streaming traffic (YouTube) after correlation with UDP/QUIC protocol statistics. In a real SOC environment, large unexplained data transfers to unfamiliar external IPs would be flagged for further investigation (possible data exfiltration).
- **Recommended Action:** Baseline normal traffic volumes per host; investigate any large transfers to IPs not associated with known/approved services.

---

## IOC 3: Dominant QUIC/UDP Protocol Usage

- **Indicator Type:** Protocol Anomaly Baseline
- **Detection Tool:** Wireshark (Statistics > Protocol Hierarchy)
- **Observation:** UDP accounted for 97.3% of packets and QUIC IETF protocol represented a notable share, consistent with modern encrypted streaming/video traffic.
- **Significance:** Establishes a traffic baseline for the network; any future capture showing an unusual spike in other protocols (e.g., unexpected ICMP floods, unusual TCP SYN ratios) can be flagged as anomalous compared to this baseline.

---

## Summary
This analysis demonstrates the ability to differentiate normal network traffic (streaming, browsing) from reconnaissance activity (port scanning) using protocol statistics, conversation analysis, and targeted display filters in Wireshark — corroborated with Nmap scan output as ground truth.
