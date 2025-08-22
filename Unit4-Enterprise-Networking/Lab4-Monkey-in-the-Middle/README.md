# Lab4-Monkey-in-the-Middle (MITM Proxy & DoS Exploration)

📌 Overview

In this lab, I explored man-in-the-middle (MITM) attacks using mitmproxy and compared encrypted network traffic between Wireshark and a proxy-based interception setup. I also experimented with intercepting and modifying network requests to understand how attackers can manipulate traffic.

Additionally, I gained exposure to how DoS/DDoS attacks affect systems and why detection & mitigation strategies are critical.

🎯 Goals
By the end of this project, I was able to:
- Capture encrypted HTTPS traffic using Wireshark.
- Set up and configure mitmproxy with Docker.
- Configure Firefox to use the proxy and install the MITM certificate.
- Intercept, analyze, and modify HTTPS requests.
- Understand DoS/DDoS attack behavior and detection.

🛠️ Steps
Exercise 0: Analyze HTTPS Traffic with Wireshark

Launch wireshark on terminal : sudo wireshark
- selected eth0 interface
- visited codepath.org and captured HTTPS traffic
- observed encrypted packets (TLS/SSL) could only see metadata

Exercise 1: Analyze HTTPS Traffic with mitmproxy

Step 0 – Install mitmproxy with Docker
  ```bash
    # Ensure Docker is installed
    sudo apt install docker.io  

    # Run mitmproxy container
    sudo docker run --rm -it \
    -v ~/.mitmproxy:/home/mitmproxy/.mitmproxy \
    -p 8080:8080 mitmproxy/mitmproxy
```
Step 1 – Configure Firefox
- Set Firefox proxy: 127.0.0.1:8080.
- Installed certificate from http://mitm.it

<img width="485" height="563" alt="network_config" src="https://github.com/user-attachments/assets/117645af-9bc7-4a7e-8ad7-075d3f393159" />

Step 2 – Capture HTTPS traffic through mitmproxy
- Revisited https://www.codepath.org/.
- Saw full HTTP headers and request details instead of just encrypted blobs.

Exercise 2: Modify Requests with mitmproxy
1. Started mitmproxy.
2. Intercepted request using curl:
   ```bash
   curl --proxy http://127.0.0.1:8080 "http://wttr.in/Dunedin?0"
3. Inside mitmproxy:
   - Pressed i → intercepted request.
   - Edited path from Dunedin → Innsbruck.
   - Resumed request.
4. Verified modified request returned weather for Innsbruck instead of Dunedin.

🔍 Key Learnings

- Wireshark can capture encrypted traffic but cannot see inside HTTPS.

- mitmproxy, when trusted as a CA, can decrypt and display HTTPS contents.

- MITM proxies demonstrate how attackers (or defenders) can intercept and modify traffic.

- Configuring certs and proxies is essential for hands-on network security testing.

- DoS/DDoS attacks emphasize the need for detection & response mechanisms in modern infrastructure.
