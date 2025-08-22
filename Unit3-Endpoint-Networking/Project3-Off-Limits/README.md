# 🚫Project 3-Off-Limits

### 📖 Overview

In this project, I investigated a simulated FTP server belonging to the Fairly Oddparents Corp. to find their real earnings by using a Directory Traversal attack. The goal was to bypass restrictions and access files hidden in restricted directories.

I created and executed a bash attack script (attack.sh) to automate the exploitation, then analyzed captured network traffic (server.pcapng) to identify which files were accessed without authorization. 

🎯 Goals
- Use bash scripting to launch a Directory Traversal attack.
- Run and extend an FTP server script with Node.js.
- Identify sensitive files accessed without proper permissions.
- Analyze .pcapng traffic captures to confirm results.

🛠️ Tools & Technologies
- FTP Protocol – file transfer & exploitation target.
- Node.js – server-side scripts (start-server.js, attack.js).
- Bash Scripting – automation via attack.sh.
- Wireshark – traffic analysis of .pcapng files.
- Vim – file editing inside terminal.

1. 🖥️ Prepare the FTP Server
     ```bash
   # Navigate to scripts folder and confirm start-server.js
    cd ftp_folder/scripts
    cat start-server.js
    # Output: var pkg = require('/usr/local/lib/node_modules/hftp');

   # Start the server manually (foreground test)
    cd ~/ftp_folder
    node scripts/start-server.js
    ```
2. ⚙️ Build & Run attack.sh
     ```bash
     # Give script executable permissions
    chmod +x attack.sh

    # Edit the script with Vim
    sudo vi attack.sh
  
3. 💥 Launch Directory Traversal Attack
     ```bash
     ./attack.sh

➡️ Retrieved hidden files outside of /general/.

4. 🔍 Analyze Captured Traffic
     ```bash
     # Open capture in Wireshark
      wireshark server.pcapng
  - follow TCP streams
  - Identify which files were accessed by the traversak attack.

✅ Results

Challenge #1: A screenshot of your completed  attack.sh  file:
<img width="704" height="513" alt="image" src="https://github.com/user-attachments/assets/556fc686-1871-4a1b-99f2-a27e70df9d49" />


Challenge #2: Three different files the Directory Traversal attack was able to access:
- Reports.txt
- /general/budget.txt
- /timmy/fishnames.txt
- /wanda/reports_original.txt

Stretch Challenge: A screenshot of your Directory Traversal attack output to find the REAL earnings
<img width="454" height="206" alt="image" src="https://github.com/user-attachments/assets/a719349e-f1f6-42a9-9d4b-037c6325dca0" />

Reflection Question: Why use a .sh file for the attack?
I use a shell script due to its automation, customization, scalability, efficiency in enumeration and avoids manual interaction that can lead to errors.

🚀 Takeaways
- I Learned how Directory Traversal attacks exploit improper input validation in file paths.
- Practiced writing bash scripts for attack automation.
- Understood how to analyze network captures to confirm attacks.
- Saw how organizations can misrepresent data and how evidence can be found in hidden files.
