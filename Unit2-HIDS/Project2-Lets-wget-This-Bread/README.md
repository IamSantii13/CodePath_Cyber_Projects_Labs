# 🍞 Project2-Lets-wget-This-Bread

### 📖 Overview

In this project, I used the Linux Audit daemon (auditd) to monitor file modifications in a protected directory. By configuring custom audit.rules, I tracked changes made by simulated attack scripts and mapped them to the affected files.

### 🎯 Goals

- Configure a set of Audit rules to monitor file changes in /protected_files.

- Launch three attack scripts to modify unknown files.

- Use ausearch to filter logs and identify which files were modified and which attack caused the change.

### 🧩 Steps & Commands

1. 📥 Download and Extract Starter Repo
    ```bash
    # Navigate to home directory
    cd ~

    # Download project repo
    wget https://github.com/codepath/project2/archive/main.zip

    # Unzip project files
    unzip main.zip

    # Enter project folder
    cd project2-main

2. 🔑 Add Permissions to Attack Scripts
   ```bash
   chmod u+x attack-a attack-b attack-c
3.⚙️ Configure Audit Rules for Monitoring /protected_files

    # Example rule: monitor writes to each file
    auditctl -w /protected_files/cloudia.txt -p w -k protect_watch
    auditctl -w /protected_files/oakley.txt -p w -k protect_watch
    auditctl -w /protected_files/precipitation.csv -p w -k protect_watch

    # Verify active rules
    auditctl -l
4. 💥 Run the Attack Scripts
     ```bash
     ./attack-a
    ./attack-b
    ./attack-c
5. 🔍 Analyze Audit Logs with ausearch
   ```bash
   # Search events related to our filter key
    ausearch -k protect_watch

    # Narrow down to writes (syscall: open, write, truncate)
    ausearch -k protect_watch -x attack-a
    ausearch -k protect_watch -x attack-b
    ausearch -k protect_watch -x attack-c

### ✅ Results
Item #1: Modified Files

- Cloudia.txt

- oakley.txt

- precipitation.csv

Item #2: File ↔ Attack Pairings

- Attack A → cloudia.txt

- Attack B → oakley.txt

- Attack C → precipitation.csv

### 🛠️ Tools & Technologies

- Linux Audit Daemon (auditd) – monitoring file system events

- ausearch – filtering audit logs

- vim – editing configuration files

- chmod – setting executable permissions

📘 Resources Used

- Vim Cheat Sheet
- Auditctl Manual
- Auditd Rules Guide

### 🚀 Takeaways

-Learned how to configure audit rules to monitor specific files.

-Practiced analyzing system logs to trace security events.

-Understood how attackers can alter files silently — and how to detect these changes with HIDS tools.
