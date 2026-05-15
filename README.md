# My SOC Analyst Portfolio & Blue Team Write-ups

Welcome to my cybersecurity portfolio! This repository contains my practical incident response reports and threat analysis write-ups from real-world simulations on the LetsDefend platform.

Completed Investigations (Case Studies)

Click on the links below to read the full incident response reports:

1. **[SOC143 - Password Stealer Detected (100% Passed)](./SOC143-Password-Stealer.md)**
Type: Malware / Trojan Execution Prevention
Outcome: True Positive. Stopped a credential theft attempt via email spoofing.

2. **[SOC142 - Multiple HTTP 500 Response (100% Passed)](./SOC142-SQL-Injection.md)**
Type: SQL Injection Attack / Web Application Security
Outcome: True Positive. Identified a database exploitation attempt via a malicious URL request.

3. **[SOC211 - Utilman.exe Winlogon Exploit Attempt (100% Passed)](./SOC211-Utilman.md)**
Type: Host Persistence / Living-off-the-land Binary (LOLBin) Mitigation
Outcome: True Positive. Defended against rogue local account creation via high-privilege system binaries.

4. **[SOC202 - FakeGPT Malicious Chrome Extension (100% Passed)](./SOC202-FakeGPT-Extension.md)**
Type: Data Leakage / Malicious Browser Add-on Analysis
Outcome: True Positive. Discovered active C2 communication and stopped potential data exfiltration channel.

Tools & Technologies Used
SIEM / Log Analysis: LetsDefend Log Management, Exchange Mail Gateway logs, Endpoint Telemetry Logs

Threat Intelligence: VirusTotal, AbuseIPDB

Documentation: Markdown, GitHub Portfolio Management

