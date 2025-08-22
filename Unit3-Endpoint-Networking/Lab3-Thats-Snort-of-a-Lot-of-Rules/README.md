# Lab3-Thats-Snort-of-a-Lot-of-Rules

### 📖 Overview
In this lab, I explored Snort, a popular Network Intrusion Detection System (NIDS), by generating test attacks with pytbull-ng, configuring Snort, and writing custom Snort rules to detect and block suspicious network activity.

### 🎯 Goals
- Install and configure Snort to monitor network traffic.
- Use pytbull-ng to simulate attacker traffic.
- Write custom Snort rules to detect and block malicious packets (e.g., ICMP & HTTPS to Google).
- Validate rule functionality with live traffic tests.

### 🛠️ Tools & Technologies

- Snort – Network Intrusion Detection System (NIDS).
- pytbull-ng – Attack simulation/testing framework.
- Docker – Containerized attacker/victim environments.
- Vim – Config file editing inside the terminal.
  
### 🧩 Steps & Commands
0. 📦 Update System & Install Docker
   ```bash
   sudo apt-get update
    sudo apt install docker.io
    sudo docker pull efigo/pytbull-ng:latest
    sudo docker images   # Verify pytbull-ng installed
   
1. 💥 Generate Attacks with pytbull-ng
    ```bash
   # Start victim container (note Host IP from output)
    sudo docker run --rm -it efigo/pytbull-ng -m victim  

   # Launch attacker container (replace <Host IP>)
    sudo docker run --rm -it efigo/pytbull-ng -m attacker -t <Host IP>
2. ⚙️ Configure Snort Environment
   ```bash
   # Create required folders
    sudo mkdir -p /usr/local/etc/rules
    sudo mkdir -p /usr/local/etc/so_rules/
    sudo mkdir -p /usr/local/etc/lists/
    sudo mkdir -p /var/log/snort

    # Create rules and blocklist files
    sudo touch /usr/local/etc/rules/local.rules
    sudo touch /usr/local/etc/lists/default.blocklist
3.  📝 Write Initial ICMP Detection Rule
     ```bash
    sudo vi /usr/local/etc/rules/local.rules

  add this line
  
    alert icmp any any -> any any ( msg:"ICMP Traffic Detected"; sid:10000001; metadata:policy security-ips alert;)

4. 🏃 Run Snort in Test & Detection Mode
   
   Test Rule syntax:
   ```bash
   snort -c /usr/local/etc/snort/snort.lua -R /usr/local/etc/rules/local.rules
  Run in detection mode (listen on eth0)
   ```bash
    sudo snort -c /usr/local/etc/snort/snort.lua -R /usr/local/etc/rules/local.rules -i eth0 -A alert_fast -s 65535 -k none
```
  Trigger the alert
  ```bash
    ping google.com
```
➡️ Snort generates ICMP alert logs.

5. 🔒 Configure snort.lua for Rule Management
   ```
   sudo vi /usr/local/etc/snort/snort.lua
update the ips section: 
  ```bash
    ips =
  {
      enable_builtin_rules = true,
      include = RULE_PATH .. "/local.rules",
      variables = default_variables
  }
```
validate config:
  ```bash
    snort -c /usr/local/etc/snort/snort.lua
```
6. 🚫 Write Rule to Detect HTTPS Traffic to Google
   edit rules file:
   ```bash
   sudo vi /usr/local/etc/rules/local.rules
  Replace ICMP rule with:
  ```bash
  alert tcp any any -> any 443 (msg:"Blocked HTTPS Traffic to Google"; sid:10000002; rev:1;)
  ```
  Run Snort again:
  ```bash
  sudo snort -c /usr/local/etc/snort/snort.lua -i eth0 -A alert_fast -s 65535 -k none
  ```
  open firefox and go to:
  https://www.google.com

➡️ Snort generates alerts for HTTPS traffic.

✅ Results
- Successfully configured Snort to detect ICMP ping traffic.
- Wrote and validated a custom rule to detect HTTPS traffic to Google.
- Used pytbull-ng to simulate real network attacks.

🚀 Takeaways
- Learned how to configure Snort rules to detect custom traffic patterns.
- Practiced simulating attacks with pytbull-ng to test IDS effectiveness.
- Gained experience in live packet analysis and intrusion detection.
- Understood how Snort can be extended with custom signatures for specific threats.
