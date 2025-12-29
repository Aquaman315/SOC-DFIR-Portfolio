1. Executive Summary
This project demonstrates the ability to identify a Brute Force Attack by analyzing Linux system authentication logs. I simulated an automated password-guessing attack against an SSH service and used command-line parsing tools (grep, awk, uniq) to isolate the source IP and quantify the scale of the attack.

2. Technical Methodology
Phase 1: Attack Simulation I used Hydra to perform a dictionary-less brute force attack against the local SSH service. This generated a high volume of "Failed Password" events in the system's security logs.

Command: sudo hydra -l root -x 3:3:a 127.0.0.1 ssh

Phase 2: Log Forensic Analysis In Linux, authentication events are stored in /var/log/auth.log. I used a "pipelined" command to extract actionable intelligence from the raw log data.

Analysis Pipeline: sudo grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr

Command Breakdown: | Tool | Purpose | | :--- | :--- | | grep "Failed password" | Filters the log to show only unsuccessful login attempts. | | awk '{print $11}' | Extracts the 11th column of the log, which contains the Attacker's IP. | | sort | Groups the same IP addresses together. | | uniq -c | Counts the number of occurrences (attempts) for each unique IP. | | sort -nr | Sorts the final list numerically from highest to lowest attempts. |

3. Results
The analysis successfully identified a high-frequency connection attempt from 127.0.0.1.

Total Attempts Detected: [INSERT YOUR COUNT HERE, e.g., 3 or 500]

Attacker IP: 127.0.0.1

Target User: root
