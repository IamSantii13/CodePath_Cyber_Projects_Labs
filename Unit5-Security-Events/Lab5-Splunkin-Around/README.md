# Lab5-Splunkin-Around

### 🔎 Overview

In this lab, I explored Splunk, a powerful SIEM (Security Information and Event Management) tool used for real-time monitoring, data analysis, and threat detection. I learned how Splunk works, built and ran searches, created dashboards, and generated reports.

Splunk is widely used in enterprise environments, especially in compliance-heavy industries like finance (e.g., PCI DSS for credit card security) and healthcare.

### 🎯 Goals
By the end of this lab, I was able to:
- Understand the Splunk interface and terminology (Index, Sourcetype, Source, Host, Time).
- Run basic Splunk searches on different datasets.
- Use stats, count, and visualizations (charts, tables, pie charts).
- Create dashboards and reports for incident monitoring.
- Identify a malicious actor in server logs.

### 🛠️Steps

Splunk Terminology
- Index – Collection of data (like a database).
- Sourcetype – Format of the data (e.g., syslog, csv).
- Source – File or stream the data came from.
- Host – Machine where data originated.
- Time – When the event occurred.

Data Sources
- Top Video Game Sales: https://www.kaggle.com/datasets/thedevastator/global-video-game-sales

Step 1: Running Basic Searches
Our first dataset comes from the Top Video Game Sales CSV file. All Splunk searches must follow the syntax:

index=MYINDEX source=MYSOURCE [other fields]

For this lab, we’re using hosts to separate the data.
In the search bar, type:

index=main host="SalesData"

- In the time drop-down, select All Time.
- Click the magnifying glass to run the search.

✅ We now see logs stored in Splunk’s main index coming from the host "SalesData".

Splunk automatically extracts fields such as Genre, Name, Publisher, Year of Release, and more. These fields allow us to refine searches.
For example, click on any Genre (e.g., Simulation, Action, Racing), then choose Add to Search.
Now our results filter down from thousands of logs to only those matching our chosen genre.
In the Interesting Fields section (left panel), you’ll see Genre=1. This confirms we’ve restricted our results to one genre.
We can keep refining:
Click on Rating, and filter to games rated E.
Add Platform and Developer of your choice to narrow down further.

Each time we update the search, the dataset shrinks to a more specific subset. Instead of “all games,” we can now search for games by Genre + Rating + Platform + Developer.
💡 Wildcard Search: Replace a field value with * to act as a wildcard. For example, changing:

Platform=*

returns results across all platforms.

Step 2: Using Stats, Count, and Creating Visualizations
Searching is useful, but Splunk becomes even more powerful when we summarize and visualize the data.

Analyze with stats: The stats command lets us calculate statistics. For example, to count the number of games by genre:

index=main host="SalesData" | stats count by Genre

Here, the pipe | connects our search to the stats function, which counts games grouped by genre.

Q: What does this search do?
👉 It shows how many games exist in each genre.

Step 2.2: Answering a Research Question
Question: Which platform has the most games released for it?
Modify the search to group by platform:

index=main host="SalesData" | stats count by Platform | sort -count

- stats count by Platform → counts games per platform.
- sort -count → sorts results from highest to lowest.
This outputs a table showing platforms ranked by game count.

Step 2.3: Visualize Results
- Switch to the Visualization tab.
- Select Column Chart.
- Change to Pie Chart for another view.

👉 However, the chart looks messy with too many platforms. Let’s clean it up:
```bash
    index=main host="SalesData" | stats count by Platform | sort -count | head 5
```
- head 5 limits results to the top 5 platforms.
- Now, switch back to a Pie Chart.

✅ Checkpoint : You should now see a pie chart showing the top 5 platforms with the most game releases.

Step 3: Using Dashboards

Dashboards help visualize incident data in Splunk. They allow analysts to create panels with charts, tables, and metrics that can be monitored in real-time.

Creating Dashboards

-  From your visualization (example: pie chart), click Save As → New Dashboard.
- Enter the following details:
  - Title: Video Game Information
  - Description: A quick view of the top video game stats.
  - Ensure Dashboard dropdown is set to New.
- Click Save.

✅ Checkpoint : Your new dashboard should open with the pie chart as the first panel.

Editing Dashboards
- Click Edit in the top-right corner.
- Add a panel title to your pie chart → "Top 5 Video Game Platforms".
- To add a new panel:
  - Click Add Panel → New Search → Statistics Table.
  - Set Time Range → All Time.
  - Run this search:
```bash
index=main host="SalesData" 
| stats count by Genre 
| sort -count
```
- Go to the Visualization tab → choose Bar Chart.
- Click Add to Dashboard, and title it: Top 10 Genres.

✅ Checkpoint : Your dashboard should now show two panels:
- A pie chart of the top 5 video game platforms.
- A bar chart of the top 10 genres.

Step 4: Creating Reports
Reports summarize data and can be shared or scheduled for delivery.

Finding the Threat Actor

- Switch to dataset:
  ```bash
  index=main host="WebServer01"
  ```
- in Interesting Fields, locate Message. Notice values like Failed Password.
- Filter failed logins:
  ```bash
  index=main host="WebServer01" Message="Failed Password for"
  ```
-Add aggregation to identify IPs with failed attempts:
  ```bash
  | stats count by IP 
| sort -count
  ```
- Go to Visualization → Bar Chart to display the top 10 IPs.

Saving the Report
- Click Save As → Report.
- Title: Top 10 Source IPs with Failed Login Attempts.
- Add a description, then Save.

🏆 Lab Reflection

This lab helped me understand how SIEM tools like Splunk can aggregate data, detect threats, and support investigations. By running searches, building dashboards, and analyzing authentication logs, I simulated how a security analyst could detect a malicious actor in real time.
