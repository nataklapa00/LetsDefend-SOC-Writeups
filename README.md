# SOC Analysis Report: SOC143 - Password Stealer Detected (EventID: 90)

## Executive Summary
* **Platform:** LetsDefend Blue Team Labs
* **Event ID:** 90
* **Date/Time:** Apr 26, 2021, 11:03 PM
* **Rule Triggered:** SOC143 - Password Stealer Detected
* **Analysis Outcome:** **True Positive**
* **Final Score:** 100% Passed

---

## Initial Alert Details & Context
The SOC monitoring system triggered a high-severity alert indicating an inbound email containing a suspected password stealing Trojan payload. 

* **Sender Address (Spoofed):** `bill@microsoft.com`
* **Source SMTP IP:** `180.76.101.229`
* **Recipient Address:** `ellie@letsdefend.io`
* **Email Subject:** `.` (Empty/Suspicious)
* **Device Action:** Allowed (Delivered to Inbox)

---

## Investigation & Playbook Steps

### 1. Artifact & Email Analysis
* **Email & Attachment Check:** Verified the inbound email metadata. The message contained a malicious attachment disguised as a legitimate communication from a trusted domain (`microsoft.com`).
* **Malware Verification:** Extracted the attachment's **MD5 Hash** and analyzed it via threat intelligence platforms (VirusTotal). The file was positively identified and flagged by multiple vendors as an active **Trojan/Password Stealer**.

### 2. Endpoint Impact Assessment
* **Delivery Confirmation:** Investigated the mail gateway logs. The email was successfully **Delivered** to the victim's mailbox (`ellie@letsdefend.io`).
* **Execution Status:** Cross-referenced host-based logs and endpoint telemetry. The malicious attachment/URL was **Not Opened** by the user. No active compromise occurred on the endpoint.

---

## Indicators of Compromise (IoCs)


| Artifact Type | Value / Indicator | Context / Comment |
| :--- | :--- | :--- |
| **Source IP** | `180.76.101.229` | Malicious SMTP server attempting email spoofing |
| **Spoofed Domain** | `bill@microsoft.com` | Pretexting / Impersonation technique |
| **File Hash (MD5)** | `[HASH containing trojan]` | Password Stealer / Trojan payload detected on VirusTotal |

---

## Containment & Remediation Recommendations
Since the malicious payload was delivered but not executed, the attack was successfully mitigated during the initial containment phase. The following incident response steps were proposed:

1. **Email Purge:** Immediately delete the malicious message from the recipient's mailbox (`ellie@letsdefend.io`) to prevent future accidental execution.
2. **Network Block:** Block the sender IP address (`180.76.101.229`) at the email gateway/firewall level.
3. **Endpoint Protection:** Blacklist and block the malicious MD5 Trojan hash across all enterprise EDR (Endpoint Detection and Response) agents.
