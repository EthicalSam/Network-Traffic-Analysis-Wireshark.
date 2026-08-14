# Network-Traffic-Analysis-Wireshark.
Wireshark based network traffic analysis and port scan detection.
# Network Traffic Analysis for Threat Detection

## Objective
Used Wireshark to capture and analyze live network traffic, identify normal traffic patterns, and detect port scanning activity performed using Nmap.

## Tools Used
- Wireshark
- Nmap
- Npcap (packet capture driver)

## Steps Performed

**1. Selected Capture Interface**
Selected the Wi-Fi interface in Wireshark to begin live traffic capture.



![Interface Selection](01%20Interface%20selection.png)



**2. Captured Live Network Traffic**
Captured live traffic while browsing (YouTube streaming), resulting in 7,754 packets.



![Live Traffic Capture](02%20Live%20Capture.png)



**3. Analyzed Protocol Distribution**
Used Statistics > Protocol Hierarchy to break down captured traffic by protocol. Identified UDP/QUIC (95.7%) as the dominant protocol, consistent with video streaming traffic, along with TCP, TLS, and DNS traffic.



![Protocol Hierarchy Statistics](03%20Protocal%20Hierarchy.png)



**4. Identified Top Conversations**
Used Statistics > Conversations (IPv4) to identify the top talkers on the network. Found a major conversation of 7,311 packets (8 MB) between the local host and a remote server, indicating a large data transfer (video stream).



![IPv4 Conversations](04%20Conversations.png)



**5. Filtered TCP SYN Traffic**
Applied the display filter `tcp.flags.syn==1 and tcp.flags.ack==0` to isolate connection initiation attempts and distinguish normal browsing behavior from scanning activity.



![SYN Filter - Normal Traffic](05%20syn%20filter%20normal.png)



**6. Performed a Controlled Port Scan**
Used Nmap's SYN Stealth Scan (`nmap -sS -v`) against the local host to simulate reconnaissance activity and identify open ports.



![Nmap SYN Stealth Scan Results](06%20nmap%20scan%20result.png)



**7. Captured and Verified Port Scanning Traffic**
Captured the scan traffic using the Wireshark Loopback Adapter and filtered with `tcp.flags.syn==1`. Identified the characteristic pattern of a port scan: multiple SYN packets sent in rapid succession to different destination ports, with SYN-ACK responses only from open ports.



![Port Scan Detected in Wireshark](07%20port%20scan%20detected.png)



## Key Learnings
- Practical experience capturing and analyzing live network traffic using Wireshark.
- Learned to use Protocol Hierarchy and Conversations statistics to profile network behavior.
- Understood how to write and apply Wireshark display filters to isolate specific traffic patterns.
- Gained hands-on understanding of how port scanning appears at the packet level (SYN packets across multiple ports, SYN-ACK from open ports only).
- Learned to distinguish normal application traffic from reconnaissance/scanning activity.
