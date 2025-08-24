# 🕵️Project5-SIEMsational-CTF SPLUNK

📌 Overview

This Capture-the-Flag (CTF) project tested my ability to search, analyze, and visualize data in Splunk. The challenges covered Netflix data exploration and an investigation into a simulated malware incident at PathCode Inc.

Goals accomplished:
- Used Splunk to query and filter structured data.
- Applied log analysis to detect malicious activity.
- Practiced creating meaningful search commands with filters, fields, and stats.

🧩 CTF Challenges
Part 1 — Netflix Data (1 pt each)

Dataset: 
```bash
index=main host=Netflix
```

Challenge 1 — How many TV shows on Netflix are in the Docuseries genre?
✅ Solution: 176
```
index=main host=Netflix type="TV Show" listed_in="Docuseries"
| stats count
```
Challenge 2 — How many movies on Netflix have a rating of TV-PG?
✅ Solution: 1080
```
index=main host=Netflix type="Movie" rating="TV-PG"
| stats count
```
Challenge 3 — How many movies on Netflix were released in the year 2020?
✅ Solution: 1034
```
index=main host=Netflix type="Movie" release_year=2020
| stats count
```
Challenge 4 — What is the longest duration by season on Netflix, and what is its TV rating?
✅ Solution: 9 seasons — TV-14
```
index=main host=Netflix type="TV Show"
| stats max(duration) as longest_duration by rating
| sort - longest_duration
```
Challenge 5 — How many movies on Netflix are listed as Action & Adventure and rated PG-13?
✅ Solution: 46
```
index=main host=Netflix type="Movie" rating="PG-13" listed_in="Action & Adventure"
| stats count
```
Challenge 6 — How many movies and TV shows on Netflix have their country of origin as Turkey?
✅ Solution: 210
```
index=main host=Netflix country="Turkey"
| stats count
```
Challenge 7 — Which release year had the most movies rated G? (Not TV-G)
✅ Solution: 2009
```
index=main host=Netflix type="Movie" rating="G"
| stats count by release_year
| sort - count
```
Challenge 8 — What two TV-Y7 shows were released in 2019 and added on November 22, 2019?
⚠️ Not completed

Challenge 9 — Which year had the most TV Shows (not Movies) from the U.S.?
✅ Solution: 2020
```
index=main host=Netflix type="TV Show" country="United States"
| stats count by release_year
| sort - count
```
Challenge 10 — What is the oldest TV show by Release Year on Netflix?
✅ Solution: 1926
```
index=main host=Netflix type="TV Show"
| stats min(release_year) as oldest_year
```
Part 2 — Investigating the Malware (2 pts each)
Dataset:
```
index=pathcode host=BluecoatProxy01 OR failedlogins64 OR uploadedhashes OR webserver02
```
Challenge 11 — What was the IP address that uploaded the malware (MD5 hash: 3AADBF7E527FC1A050E1C97FEA1CBA4D)?
✅ Solution: 192.168.1.10
```
(host=uploadedhashes OR host=webserver02) "3AADBF7E527FC1A050E1C97FEA1CBA4D"
| table _time host src_ip md5 file_name
```
Challenge 12 — What usernames did that IP try to login as? Which one did they upload a file as?
⚠️ Not completed

Challenge 13 — What was the User Agent String of the attacker during upload?
✅ Solution: Opera/75.0.3969.218
```
(host=uploadedhashes OR host=webserver02) "3AADBF7E527FC1A050E1C97FEA1CBA4D" host=uploadedhashes
```
📌 Takeaways
- Practiced structured Splunk searches with multiple filters.
- Learned how to analyze log data for anomalies (failed logins, malicious uploads).
- Reinforced the value of fields like IP addresses and user agents in identifying attackers.
