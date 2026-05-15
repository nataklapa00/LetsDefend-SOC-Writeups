# My SOC Investigation: SOC167 - LS Command Detected in Requested URL (EventID: 117)

I completed this incident investigation on the LetsDefend platform, scoring a 100% success rate. The alert was triggered by a potential OS Command Injection signature detected within an HTTP GET request string. After analyzing the evidence and network context, I confirmed this case as a False Positive.

Target Hostname: EliotPRD

Source IP Address: `172.16.17.46`

Destination IP Address: `188.114.96.15`

Requested URL: `https://letsdefend.io/blog/?s=skills`

# How I Investigated This Incident

# Step 1: Investigating the URL Parameters & Signature Match
First, I inspected the HTTP telemetry and the exact query string that triggered the alert. The security rule flagged the request because it matched a signature for the Linux directory listing command (`ls`). However, looking closely at the requested parameter `?s=skills`, the letters "ls" were simply part of the legitimate English word "skills" typed into the blog search bar.

# Step 2: Analyzing Network Context & Traffic Legitimacy
Next, I evaluated the source and destination behavior to check for automated scanning or malicious intent. The user-agent indicated a standard web browser, and the request path was pointed toward a standard search engine feature of the blog. 

Since the string was part of normal user input and not an attempt to escape the application context to execute OS commands, the traffic was determined to be completely benign. The device action was "Allowed" correctly because no security violation occurred.

Indicators of Compromise (IoCs)

 Legitimate Search Query: `?s=skills` (False match on "ls" signature)
 
Verdict: No malicious indicators found; normal user activity.

My Recommendations for Tuning
Since the activity was legitimate and did not pose a threat to the application, I submitted the following tuning steps to prevent future false alarms:
1. Optimize SIEM/WAF Signatures: Refine the regex rule for the `ls` command detection so it requires whitespace, semicolons, or pipe characters around the command rather than triggering on substrings inside normal words.
2. Close as False Positive: Clear the alert from the queue with no further escalation or host isolation required.
