# 🛡️ Project 4 – DoS Attack & Mitigation with NGINX

📌 Overview
In this project, I performed a Denial of Service (DoS) attack using Slowloris against a local VM server, and then implemented DoS mitigation rules in NGINX to protect against the attack. Finally, I compared .pcap files in Wireshark to analyze the difference between a vulnerable and protected server.

## Steps
1. Verified tools are working
  I first checked if nginx and slowloris were installed:
  ```bash
  nginx -v
  slowloris -h
```
  Stopped Apache since it sometimes auto-starts after updates:
   ```bash
    sudo service apache2 stop
```
Checked if port 80 was free:
  ```bash
      lsof -i :80
```
2. 🚀 Set up NGINX Monitoring with Amplify
  Followed the nginx Amplify Agent guide. Main steps included:
  ```bash
  # Update system
sudo apt-get update && sudo apt-get upgrade -y  

# Install amplify-agent
sudo apt-get install amplify-agent  

# Configure stub_status
sudo vi /etc/nginx/conf.d/stub_status.conf
```
  inserted monitoring config in nginx:
  ```bash
server {
    listen 127.0.0.1:8080;
    location /nginx_status {
        stub_status;
        allow 127.0.0.1;
        deny all;
    }
}
```
  Reloaded nginx:
  ```bash
sudo systemctl start nginx
```
  Started nginx:
  ```bash
sudo systemctl start nginx
```

3. 🧨 Ran Slowloris (Pre-Mitigation)
   I launched Slowloris to simulate an attack against the local nginx server:
   ```bash
   slowloris 127.0.0.1
This was left running for ~10 minutes. On the Amplify Dashboard, I noticed:
- A large number of open connections.
- Requests were stuck, showing typical Slowloris behavior.

4. 🔒 Applied DoS Mitigation Rules in NGINX
  Edited the nginx config file:
  ```bash
  sudo vi /etc/nginx/conf.d/default.conf
```
  Mitigation rules added in nginx: 
  ```bash
server {
    listen 80 default_server;

    # Limit connections
    limit_conn_zone $binary_remote_addr zone=addr:10m;
    limit_conn addr 10;

    # Limit request rate
    limit_req_zone $binary_remote_addr zone=req_limit_per_ip:10m rate=1r/s;

    server_name localhost;

    location / {
        limit_conn addr 10;
        limit_req zone=req_limit_per_ip burst=5 nodelay;
        root /usr/share/nginx/html;
        index index.html;
    }
}
```
  Restarted nginx: 
  ```bash
  sudo systemctl restart nginx
```

5. 🧨 Ran Slowloris (Post-Mitigation)
   I ran slowloris again:
   ```bash
   slowloris 127.0.0.1
    ```
On the Amplify dashboard, I saw:
- Fewer connections sustained.
- Requests timed out quickly.
- The attack no longer degraded server performance.

6. 🔍 PCAP Analysis in Wireshark
I opened both .pcap files and compared packet traffic:

Vulnerable Server (no mitigation):
- High number of SYN packets with delayed ACKs.
- Connections remained half-open.
- Very few RST packets.

Mitigated Server (with nginx rules):
- Frequent RST packets (nginx dropping malicious connections).
- New connections limited after a threshold.
- Healthier TCP flow overall.

Item #2: A detailed explanation (two sentences minimum) of how you know that your DoS mitigation rules are working:
The DoS mitigation rules are working because the connection and request limits in the Nginx configuration prevent Slowloris from keeping a large number of connections open indefinitely. This can be confirmed by monitoring the Amplify dashboard and Nginx logs, which show a reduced number of active connections and entries for dropped or rejected requests, meaning the server remains stable instead of becoming overwhelmed.

Item #3: A detailed explanation of how you know which .pcap file is from the vulnerable server, and which is from the server with DoS mitigation set up:
The .pcap file from the vulnerable server shows many simultaneous TCP connections to port 80 that remain half-open or idle for long periods, which is characteristic of a successful Slowloris attack. The lack of RST (reset) or server-side termination packets indicates that the server is not defending itself effectively.
In contrast, the .pcap file from the server with mitigation shows fewer sustained connections, with many connections being reset or closed quickly after the handshake. This indicates that the mitigation rules (rate limiting, connection caps, or timeouts) are working to prevent Slowloris from exhausting server resources.
