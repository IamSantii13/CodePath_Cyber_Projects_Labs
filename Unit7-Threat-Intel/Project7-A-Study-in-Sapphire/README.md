# Project7-A-Study-in-Sapphire

# 🔎 Project 7 - A Study in Sapphire

## 📖 Overview
This project focuses on **threat hunting and digital forensics** using real-world techniques.  
The goal was to investigate Indicators of Compromise (IOCs) from the **SolarWinds attack**, analyze network logs in **Splunk**, and validate findings through **VirusTotal**.  

The project simulates the role of a **Threat Hunter**, piecing together log data to identify malicious activity and building Splunk dashboards to monitor potential compromises.

---

## 🎯 Key Objectives
- Apply threat intelligence to identify and analyze SolarWinds IOCs.
- Search and correlate network logs for signs of compromise in Splunk.
- Research malicious IPs with VirusTotal.
- Build a Splunk dashboard for monitoring IOC-related events.
- (Optional) Extend detection by investigating third-party threat feeds (e.g., Log4J, WannaCry).

---

## 🛠️ Tools & Technologies
- **Splunk** – log ingestion, searches, dashboards
- **MISP (Malware Information Sharing Platform)** – threat intelligence
- **VirusTotal** – IP/domain reputation checks
- **CSV log files** – simulated proxy/network traffic

---

## 📊 Findings

### ✅ Match #1
- **IP Address:** `13.59.205.66`  
- **Date/Time:** 2024-03-04 06:57:28  
- **Affected Computer:** `WS-SolarWave-212`  
- **Evidence:** IOC match in Splunk, confirmed in VirusTotal  
<img width="702" height="211" alt="image" src="https://github.com/user-attachments/assets/6f636325-e58e-4c4c-84c3-3dd11acc868e" />
<img width="684" height="313" alt="image" src="https://github.com/user-attachments/assets/7118fe83-f966-4bd4-a9e0-558ebbb6c4e5" />

### ✅ Match #2
- **IP Address:** `54.215.192.52`  
- **Date/Time:** 2024-03-05 07:10:28  
- **Affected Computer:** `LN-SolarShadow-552`  
- **Evidence:** IOC match in Splunk, confirmed in VirusTotal  
<img width="703" height="211" alt="image" src="https://github.com/user-attachments/assets/c1a6a9dd-40cd-4dfb-a537-7bfbce73acc6" />
<img width="664" height="286" alt="image" src="https://github.com/user-attachments/assets/e5768979-fc26-48f6-9512-8d882368cb24" />

### ⭐ Stretch Challenge (Bonus)
- **IP Address:** `5.252.177.25`  
- **Date/Time:** 2024-03-05 07:11:28  
- **Affected Computer:** `LN-SolarStrike-14`
<img width="1045" height="309" alt="image" src="https://github.com/user-attachments/assets/918bac6c-28f7-4f3f-9de9-a794447a0f11" />
<img width="1068" height="586" alt="image" src="https://github.com/user-attachments/assets/4e8e3d90-00dc-43bc-bd9a-ebe651374115" />

---

## 📈 Splunk Dashboard Query
```spl
| inputlookup SolarWindsIOCs.csv
| search Indicator_type=ip
| rename "IP Address" as ioc_ip
| join type=inner ioc_ip [
    search source="NetworkProxyLog02.csv"
    | rename "IP Address" as ioc_ip
]
| table Date, Time, "Computer Name", "Source IP", ioc_ip, Note

```
🤔 Reflection

What is an IOC? 🛑🚨☎️

Would I trust a previously malicious IP?
No — like hiring an employee with a robbery record, the IP could still pose risks.

What I learned: 
- How to combine threat intel with log analysis
- validate malicious activity
- build visual monitoring in Splunk.

🚀 Skills Demonstrated
- Threat Hunting
- Digital Forensics
- Log Analysis
- Splunk Dashboards
- Threat Intelligence Integration
