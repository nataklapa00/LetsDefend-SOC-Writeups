# My SOC Investigation: SOC143 - Password Stealer (EventID: 90)

Quick Overview
I completed this incident investigation on the LetsDefend platform, scoring a 100% success rate. The alert was triggered by an inbound email carrying a dangerous password-stealing Trojan. After analyzing the evidence, I confirmed this case as a **True Positive**.

* **Sender (Spoofed):** bill@microsoft[.]com
* **True Source IP:** 180.76.101[.]229
* **Target Mailbox:** ellie@letsdefend[.]io
* **Email Subject:** `.` (Highly suspicious single dot)

---
How I Investigated This Incident

### Step 1: Checking the Email & Malware Payload
First, I looked into the email headers and the attachment. The attacker used email spoofing to pretend they were writing from a trusted Microsoft domain. I extracted the attachment's MD5 Hash and checked it against VirusTotal. Multiple security vendors flagged the file as an active Trojan / Password Stealer.

### Step 2: Checking Host Impact
Next, I needed to check if the network was already compromised. I analyzed the mail gateway logs and confirmed that the email was successfully Delivered to Ellie's inbox. 

However, after verifying the endpoint logs and system telemetry, I found that the user did not open or execute the file. This means the threat was contained before it could infect the machine.

---
Indicators of Compromise (IoCs)
* Malicious Server IP: `180.76.101.229` (The actual server used to send the spam)
* Spoofed Address: `bill@microsoft.com`

---
My Recommendations for Containment
Even though the user didn't run the file, the malicious artifact is still sitting in the inbox. I submitted the following remediation steps to close the case:
1. Purge the Email: Delete the message from `ellie@letsdefend.io` immediately so no one clicks it by accident.
2. Block the Sender: Add the source IP `180.76.101.229` to the firewall/mail gateway blocklist.
3. Blacklist the Hash: Push the malicious MD5 hash to our EDR/Antivirus solution to block it enterprise-wide.
