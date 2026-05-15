# My SOC Investigation: SOC202 - FakeGPT Malicious Chrome Extension (EventID: 153)

I completed this incident investigation on the LetsDefend platform, scoring a 100% success rate. The alert was triggered by a suspicious browser extension file (`.crx`) loaded into Google Chrome. After analyzing the evidence, I confirmed this case as a True Positive.

Target Hostname: Samuel

Host IP Address: `172.16.17.173`

Malicious File Path: `C:\Users\LetsDefend\Download\hacfaophiklaeolhnmckojjjjbnappen.crx`

Trigger Command Line: `chrome.exe --single-argument C:\Users\LetsDefend\Download\hacfaophiklaeolhnmckojjjjbnappen.crx`

# How I Investigated This Incident

# Step 1: Investigating the Extension Payload
First, I inspected the browser telemetry and the dropped file details. The command line shows Google Chrome loading an external extension package disguised as a ChatGPT tool ("FakeGPT"). I verified the file hash and confirmed it was active malware designed to steal sensitive browser data. Because the system action was logged as "Allowed", the malicious extension successfully integrated into the user's browser.

# Step 2: Checking Network & System Impact
Next, I analyzed the network logs to evaluate data exposure. I found clear evidence that the host established a connection to the attacker's Command and Control (C2) server. 

Endpoint protection logs confirmed the extension was Not Quarantined during execution. This access pattern indicates an active Data Leakage scenario where user session tokens, credentials, or browsing data were actively exfiltrated.

Indicators of Compromise (IoCs)

 Malicious File Hash: `7421f9abe5e618a0d517861f4709df53292a5f137053a227bfb4eb8e152a4669`
 
Extension ID / Name: `hacfaophiklaeolhnmckojjjjbnappen.crx`

My Recommendations for Containment
The system is compromised and actively communicating with external infrastructure. I submitted the following remediation steps to close the case:
1. Isolate the Host: Disconnect the hostname `Samuel` (`172.16.17.173`) from the local network immediately to cut off the C2 data exfiltration channel.
2. Purge the Malicious Files: Completely delete the `.crx` installer file and remove the corresponding extension directory from the Chrome user profile.
3. Block the Hash: Add the malicious file hash to the enterprise EDR blocklist to prevent execution on other endpoints.
