# Project6-CSIRT-PathCode

# 🚨 Project 6 - CSIRT: Pathcode

## 📌 Overview
In this project, I stepped into the role of a **cybersecurity analyst** as part of a Computer Security Incident Response Team (CSIRT).  
The challenge combined **Splunk** (for malware detection) with **Catalyst** (for incident documentation).  
The goal was to investigate Indicators of Compromise (IOCs), document artifacts, and create a structured incident response case.  

Bonus features included analyzing real-world breaches, attacker methods, and organizational responses — deepening the understanding of cybersecurity threats and IR frameworks.  

---

## 🎯 Goals
By completing this project, I was able to:  
- Use **Splunk** to detect and investigate malware activity.  
- Use **Catalyst** to document the incident response process.  
- Add artifacts to Catalyst and enrich them with external sources like **VirusTotal** and **AbuseIPDB**.  
- Write up **Findings** and **Lessons Learned** from the incident.  

---

## 🧩 Tasks
### Required: Malware Investigation with Splunk & Catalyst
- Investigated a fictional malware upload to a company server.  
- Used Splunk logs (`index=pathcode`) to trace authentication data, antivirus alerts, and file uploads.  
- Documented findings in Catalyst, tracking artifacts and associated Tactics, Techniques, and Procedures (TTPs).  
- Researched IOCs in external sources (VirusTotal, AbuseIPDB).  
- Wrote a Lessons Learned report aligned with **NIST/SANS IR framework**.  

---

## ✅ Deliverables

### Item #1: Splunk Malware Case in Catalyst
<img width="704" height="450" alt="image" src="https://github.com/user-attachments/assets/d5add87d-b991-403e-aec4-051c4c2eb1cf" />


---

### Item #2: Artifact & External Research
**Artifact Investigated:** `192[.]168[.]1[.]10`  
- Suspicious executable: `Evil.exe` uploaded (found in `/home/codepath/Files/Splunk-5-6-7/uploadedhashes.csv`).  
- IP is private → no AbuseIPDB result.  
- VirusTotal flagged the file with suspicious **UDP network behavior**.  
- This suggests the malware may use C2 communication patterns.  
- Artifact was de-fanged (`192[.]168[.]1[.]10`) and added to Catalyst for tracking.  

<img width="700" height="444" alt="image" src="https://github.com/user-attachments/assets/7ec2b9de-0311-480c-9712-fb49723c552a" />


---

### Item #3: Findings & Lessons Learned
**Findings:**  
- Suspicious file upload event linked to `Evil.exe`.  
- VirusTotal shows potentially novel or evolving threat indicators.  
- Indicates adversary leveraging new tactics to bypass detection.  

**Lessons Learned:**  
1. **Detection & Monitoring**  
   - Enhanced Splunk logging surfaced the event.  
   - Future steps: configure **real-time alerts** for suspicious uploads & abnormal network traffic.  

2. **Prevention**  
   - Implement **file upload restrictions** (executables blocked by default).  
   - Consider **application whitelisting**.  
   - Deploy **EDR tools** for behavioral detection.  

3. **Response & Containment**  
   - Host `192.168.1.10` should be **isolated immediately** during Incident Response.  
   - Flag suspicious IPs/hashes for **automated correlation** in SIEM.  

4. **Policy Improvements**  
   - Mandate **sandboxing for all uploaded executables**.  
   - Increase frequency of threat intelligence pulls (even for internal IPs).  

---

## 🤔 Reflection

**Reflection Q1:** If I had to explain “what is a cyber breach” in 3 emojis:  
‼️☎️🚨  

**Reflection Q2:** The most important step of the IR process:  
➡️ **Preparation**, since it’s the foundation for everything.  


---

## ⚙️ Technical Notes
Some of the commands/log queries I used:  

```bash
# Splunk search for malware-related file uploads
index=pathcode sourcetype=csv uploadedhashes.csv Evil.exe

# Investigate network activity
index=pathcode sourcetype=netflow dest_ip=192.168.1.10

# Research IOC hash in VirusTotal (manual step)
curl --request GET \
     --url 'https://www.virustotal.com/api/v3/files/<HASH>' \
     --header 'x-apikey: <API_KEY>'
