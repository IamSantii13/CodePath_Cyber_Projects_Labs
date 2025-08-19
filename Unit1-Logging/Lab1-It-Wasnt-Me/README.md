# Lab1-It-Wasnt-Me

##Overview

In this lab, I played the role of a Blue Team analyst in the Security Operations Center (SOC) at Boring Office. On April 19th, 2023, at 12:50 PM, an employee allegedly sent sensitive information to the entire company. However, the employee denied involvement and claimed someone impersonated them. My task was to investigate the incident and determine which employee was truly responsible. This lab focused on **logging and event analysis** to investigate suspicious network activity. Using Wireshark, I conducted a compact incident investigation by correlating **network packet data (PCAP)**, **DHCP lease records**, and **host security logs**. Evidence shows the email was sent from **IP `10.10.1.4`**, which DHCP assigned to **host `USER2`** shortly before the incident. Host security logs indicate **John Doe** was logged into `USER2` at the relevant time, aligning with the transmission window. Attribution: **John Doe on USER2**.

---

## 🎯 Objectives
- Gain hands-on experience with **Wireshark** for traffic inspection.  
- **Inspect PCAP files** to extract relevant email communication.  
- Correlate the IP address with a host device using DHCP logs.  
- Use system security logs to determine which employee was logged in at the time of the incident.
---

## 🗂️ Evidence & Inputs
- **`SMTP.pcap`** — packet capture of email traffic.
- **`DHCP.txt`** — DHCP server events/leases.
- **`Security_log.rtf`** — host security logins/logoffs.
  
*(Reference concepts: SMTP, IP addressing (Network Layer), DHCP)*

---
## 🛠️ Tools & Techniques
- **Wireshark** → For packet capture analysis.  
- **Email Traffic Analysis** → Inspected SMTP, POP3, and IMAP packets.  
- **Event Correlation** → Connected log entries with packet behavior.  

---


## 🔍 Investigation Summary
1. Loaded the provided PCAP file into **Wireshark**.  
2. Applied relevant filters (e.g., `smtp`, `tcp.port == 25`) to isolate email traffic.  
3. Identified anomalies within the packet stream, including questionable attachments.  
4. Correlated timestamps and activity to assess whether the traffic matched normal behavior.  

---
## 🔬 Methodology
1. **PCAP triage (Wireshark)**  
   Applied focused filtering on SMTP traffic to isolate sender activity and metadata.
   - Display filters used:
     - `smtp`
     - `smtp contains "FROM"`

2. **IP–Host correlation (DHCP)**  
   Reviewed DHCP events immediately **preceding 12:50 PM** to locate the lease associated with the source IP observed in the PCAP.

3. **User attribution (Security Log)**  
   Matched the identified host to its **interactive logon** sessions, confirming which user account was active during the transmission window.

---

## 🔍 Analysis Details

### 1) Source IP from PCAP
- Filtering to SMTP traffic (`smtp`) narrowed the capture to mail flows.
- Refining with `smtp contains "FROM"` isolated the sender declaration.
- **Result:** The SMTP transaction that contained the broadcast email originated from **source IP `10.10.1.4`**.

### 2) IP → Host via DHCP
- Focus window: events just before **12:50 PM** (email send time).  
- At **12:11:27 PM**, DHCP assigned **`10.10.1.4`** to **host `USER2`**.  
- **Result:** The device using the source IP at the incident time was **`USER2`**.

### 3) User on Host via Security Log
- The security log shows four events:
  - A logon/logoff session on **`USER1`** by **Jane Doe** (not relevant to the source IP).
  - A logon/logoff session on **`USER2`** by **John Doe**.
- The **`USER2` / John Doe** session **overlaps** with **12:50 PM** (email send time).  
- **Result:** **John Doe** was the logged-in user on the implicated host when the email was sent.

---

## 🗓️ Consolidated Timeline
| Time (Local)     | Event                                                     | Source          |
|------------------|-----------------------------------------------------------|-----------------|
| 12:11:27 PM      | DHCP assigns **10.10.1.4** to **`USER2`**                 | `DHCP.txt`      |
| ~12:50 PM        | Company-wide sensitive email transmitted (SMTP)           | `SMTP.pcap`     |
| Around incident  | **John Doe** logged on to **`USER2`** (overlapping window)| `Security_log.rtf` |

---

## 📑 Findings
- **Source IP:** `10.10.1.4` (from SMTP PCAP).  
- **Assigned Host:** `USER2` (from DHCP at **12:11:27 PM**).  
- **Logged-in User on Host:** **John Doe** (overlapping with **12:50 PM** send time).  

**Attribution:** Email sent from **`USER2`** while **John Doe** was logged in.

---


## ✅ Conclusion
All three evidence streams independently align:
1) PCAP fixes the **network origin** (`10.10.1.4`),  
2) DHCP ties that **IP to a host** (`USER2`), and  
3) Host security logs place a **specific user on that host** (John Doe) **at the critical time**.  
**Conclusion:** The broadcast email was sent from **USER2** by **John Doe**.

---

## 🛡️ Recommendations
- **Account Controls:** Enforce MFA on email accounts; review delegated send/as permissions.
- **Endpoint Telemetry:** Centralize host log collection; alert on unusual SMTP patterns.
- **Network Monitoring:** Add SMTP egress rules and detections for bulk internal distribution.
- **DHCP & Identity Hygiene:** Ensure precise time sync (NTP) across DHCP, endpoints, and SIEM for reliable cross-log correlation.

---

## 📎 Appendix: Key Filters / References
- **Wireshark:**  
  - `smtp`  
  - `smtp contains "FROM"`  
- **Artifacts consulted:** `SMTP.pcap`, `DHCP.txt`, `Security_log.rtf`

```wireshark
Filter Used: smtp || pop || imap
