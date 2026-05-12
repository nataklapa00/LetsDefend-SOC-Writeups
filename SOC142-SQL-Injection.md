# My SOC Investigation: SOC142 - Multiple HTTP 500 Response (EventID: 89)

I investigated an incident on the LetsDefend platform where a high-severity alert triggered due to multiple HTTP 500 server errors. My analysis confirmed that an external attacker was attempting an SQL Injection attack against our internal SQL database server. The investigation was completed with a 100% success rate, and I classified the alert as a True Positive.

Alert Rule: SOC142 - Multiple HTTP 500 Response

Attack Date and Time: Apr 18, 2021, 01:00 PM

Attacker IP: 101.32.223.119

Target Server IP: 172.16.20.6 (SQLServer)

Compromised Account: www-data

Firewall Action: Allowed


How I Investigated This Incident

#Step 1: Analyzing the Traffic and URL
I began by examining the web proxy logs associated with EventID 89. The logs showed that an external IP address, 101.32.223.119, sent a highly suspicious web request to our internal SQLServer. 

The exact requested URL was: https[:]//172[.]16.20[.]6/userNumber=1 AND (SELECT * FROM Users) = 1

Looking closely at this URL, the attacker appended a database query payload to the end of the standard parameter. They were trying to manipulate the web application to extract sensitive data straight from our database tables. This is a classic textbook SQL Injection attempt.

#Step 2: Checking Server Impact
Next, I reviewed the alert context. The rule triggered because the server started returning multiple HTTP 500 Internal Server Error codes. This indicates that the attacker's web requests were successfully hitting the backend application, causing the web server to fail or leak information while processing the bad SQL queries. 

The logs confirmed that the attacker successfully accessed the application. Because the local firewall action was set to Allowed, the attack traffic was not stopped automatically by our network devices.


Indicators of Compromise (IoCs)

Attacker Source IP: 101.32.223.119

Target System: 172.16.20.6 (SQLServer running as user www-data)

Attack Pattern: SQL Injection payload inside the HTTP GET request URL


My Recommendations for Containment and Remediation
To mitigate this threat and secure the infrastructure from future database attacks, I proposed the following remediation steps:

1. Network Blocking: Immediately ban and block the malicious source IP address 101.32.223.119 at the perimeter firewall to stop further communication.
2. Web Application Firewall (WAF): Implement a Web Application Firewall in front of the server. A WAF can inspect incoming web traffic and automatically block SQL Injection strings before they ever reach the backend database.
3. Code Patching: Inform the development team to implement proper input validation and parameterized queries in the web application to fix the underlying vulnerability.
