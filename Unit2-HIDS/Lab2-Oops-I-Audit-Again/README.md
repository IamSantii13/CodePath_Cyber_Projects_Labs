# 🛡️ Lab 2 – Oops!...I Audit Again

## 📖 Overview
In this lab, I explored **Host Intrusion Detection Systems (HIDS)**, which monitor endpoints (like computers or servers) for suspicious activity. Specifically, I used the **Linux Audit daemon (`auditd`)** to detect file modifications and log security-related events.  

This hands-on exercise introduced me to:  
- Setting up `auditd` on my VM  
- Using **Vim** to create and edit files  
- Writing custom **audit rules**  
- Sort through the /var/log events for audit findings 

By the end, I was able to detect when files were changed on my system and trace those changes through the audit logs.  

---

## 🎯 Goals
- ✅ Install and configure the Linux Audit daemon  
- ✅ Gain familiarity with Vim text editor  
- ✅ Write `audit.rules` to detect file modifications  
- ✅ Use filter keys to sort through `/var/log/audit/audit.log`  

---

## 🛠️ Lab Steps

### 🔹 Exercise 0: Installing Audit
1. Updated package sources:  
   ```bash
   sudo apt-get update
2. Installed Audit daemon:
   ```bash
   sudo apt-get install auditd
3. verified it was running:
     ```bash
     sudo systemctl status auditd
✅ Checkpoint: Audit daemon installed successfully!

---

### 🔹 Exercise 1: Writing Rules
1. Created a file to monitor:
    ```bash
    touch unit2_lab.txt
    
2. Opened file in Vim and added text (vi unit2_lab.txt).
3. Edited /etc/audit/rules.d/audit.rules with superuser privileges:
     ```bash
     sudo vi /etc/audit/rules.d/audit.rules
4. Added custom monitoring rule:
   ```bash
   -w /home/codepath/unit2_lab.txt -p w -k unit2_lab_changes

- -w → watch the file

- -p w → detect writes (modifications)

- -k → tag with filter key for searching logs

5. Restart Audit service
   ```bash
   sudo systemctl restart auditd
✅ Checkpoint: File monitoring rule applied!

---

### 🔹 Exercise 2: Viewing Logs
1. Modified the file with Vim
   ```bash
   sudo vi unit2_lab.txt
2. To confirm I logged an event lets check the raw logs with:
   ```bash
   sudo vi /var/log/audit/audit.log
<img width="1684" height="490" alt="logs" src="https://github.com/user-attachments/assets/d823cc5d-5e44-4ff5-a955-ab882ab3d05e" />

that is too much! lets filter them instead.

3. Filter logs with custom key
   ```bash
   sudo ausearch -ts today -k unit2_lab_changes
<img width="967" height="276" alt="ausearch_command" src="https://github.com/user-attachments/assets/0213c986-831e-41f9-8b10-6c05b0b5fcac" />

This displayed two types of events:

Rule creation event → exe="/usr/sbin/auditctl" (when the rule was added)

File modification event → exe="/usr/bin/vim.basic" (when the file was edited)

✅ Checkpoint: Successfully detected suspicious file activity!

🧠 Reflection

Q1: What did I learn about HIDS from this lab?
HIDS like auditd are powerful because they allow defenders to monitor specific files or actions without needing a pre-built detection ruleset. This makes it possible to quickly spot tampering or unauthorized access on critical files.

Q2: How does using keys (-k) make auditing easier?
Filter keys act like labels in the logs, allowing us to quickly extract only the events we care about instead of digging through thousands of raw entries.


