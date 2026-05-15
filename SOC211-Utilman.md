# My SOC Investigation: SOC211 - Utilman.exe Winlogon Exploit Attempt (EventID: 161)

I completed this incident investigation on the LetsDefend platform, scoring a 100% success rate. The alert was triggered by an unauthorized account creation command executed via the Windows Utility Manager (`utilman.exe`). After analyzing the evidence, I confirmed this case as a True Positive.

Target Hostname: Henry

Host IP Address: `172.16.17.149`

Trigger Process: `Utilman.exe` (Parent: `Winlogon.exe`)

Malicious Command Line: `net user superman onepunch123 /add`

# How I Investigated This Incident

# Step 1: Analyzing the Binary & Trigger Reason
First, I reviewed the alert details and process telemetry. The alert was triggered because a command-line operation was launched directly from `Winlogon.exe` using `Utilman.exe`. This specific behavior indicates a well-known Living-off-the-land binary (LOLBin) exploitation technique, where attackers abuse built-in Windows accessibility features at the login screen to gain unauthorized system access.

# Step 2: Checking Host Impact & Attack Objective
Next, I analyzed the exact command line executed on the host: `net user superman onepunch123 /add`. The attacker used the compromised binary to create a new local user account named "superman" with the password "onepunch123". 

Because the endpoint protection device action was set to "Allowed", the command successfully executed on Henry's machine. The attacker's clear objective here was establishing long-term Persistence on the endpoint. 

Indicators of Compromise (IoCs)

 Malicious Process Hash (MD5): `ded8fd7f36417f66eb6ada10e0c0d7c0022986e9`
 
Unauthorized Account Created: `superman`

My Recommendations for Containment
Since the malicious command was allowed to execute, the host is actively compromised. I submitted the following remediation steps to close the case:
1. Isolate the Host: Disconnect the hostname `Henry` (`172.16.17.149`) from the network immediately to prevent potential lateral movement.
2. Remove the Rogue Account: Delete the unauthorized local user account `superman` from the system configuration.
3. Revert Registry Modifications: Inspect and clean the `Image File Execution Options` (IFEO) registry keys to remove the `utilman.exe` persistence hook.
