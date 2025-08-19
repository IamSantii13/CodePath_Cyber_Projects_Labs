# 🕵️ Project 1 - Catch Me If You Can  

## 📖 Overview  
This project focused on analyzing **Business Email Compromise (BEC)** scams through captured email traffic in `.pcap` files. Using **Wireshark**, I examined SMTP packets to separate **legitimate** emails from **phishing** ones and ultimately identified the malicious actor behind the scam attempt.  

BEC scams are extremely costly in the real world (over **$26 billion in losses between 2016–2019**) and highlight the importance of email traffic analysis for **incident response and threat detection**.  

---

## 🎯 Goals  
By completing this project, I learned how to:  
- Inspect `.pcap` files and extract raw email content  
- Apply Wireshark **display filters** to isolate suspicious packets  
- Differentiate **legitimate vs. fraudulent** emails  
- Identify the **IP address** of a malicious actor  

---

## 🔎 Process & Methodology  

1. **Packet Capture Review**  
   - Loaded the provided `pcap_files.zip` into Wireshark.  
   - Focused specifically on SMTP protocol traffic since it handles email communication.  

2. **Filtering Emails**  
   - Used the following key Wireshark filters to narrow down suspicious activity:  
     ```wireshark
     smtp contains "FROM"
     smtp.data
     smtp.req.parameter
     ```  
   - This allowed me to isolate emails with suspicious senders and unusual subjects.  

3. **Email Analysis**  
   - Examined email subjects and metadata for **phishing red flags** (urgent language, threats, or strange codes).  
   - Confirmed fraudulent intent based on subject patterns and repeated abnormal senders.  

4. **Actor Identification**  
   - From the filtered results, the **malicious sender’s IP address** was consistently observed as:  
     ```
     10.6.1.104
     ```  

---

## ✅ Required Challenges  

**Item 1: Malicious IP Address**  
-10.6.1.104

**Item 2: Three Phishing Email Subject Lines**  
- `Subject: Pay! - 12345`  
- `Subject: I won't warn you again! - saa124chel`  
- `Subject: Take care next time! - sabilala`  

**Item 3: Explanation of How Actor Was Found**  
I used the filter `smtp contains "FROM"` in Wireshark to identify email traffic. Once the emails were isolated, I reviewed the data fragments in the **Packet Details** pane. By correlating the suspicious subject lines with the source IP address, I confirmed the malicious actor as `10.6.1.104`.  

---

## ✨ Reflection  

** How does Wireshark help us to analyze network traffic?**  
Wireshark acts as a microscope for network traffic. It captures live or saved packets, breaks them down by protocol, and allows analysts to filter, inspect, and reconstruct activities such as email communications.  

---
