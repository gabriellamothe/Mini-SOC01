# SOC Tier 1 Triage Report

## Incident Details
- **Alert ID**: 1
- **Timestamp**: 14:32:17
- **Source IP**: 192.168.1.105
- **Destination IP**: 10.10.1.25
- **Severity**: high

## Summary of Findings
[Provide brief overview of what was discovered]
I found that in the firewall logs the attacker was approved to go through and was able to start logging in for the database and tried many usernames and passwords and while other users logged in the ids started to realize the attacker was guessing when the attacker started trying to login as an admin and the login wasnt working and the attacker repeated guessing and the ids caught on at 14:32:17 and detected that an sql injection was occuring.
## Evidence Collected
2024-01-15 14:32:17 User login attempt from 192.168.1.105
User: admin
Status: Failed
2024-01-15 14:32:17 192.168.1.105 10.10.1.25 TCP 80 14:32:17 ESTABLISHED
2024-01-15 14:32:17 [123456789] SQL Injection Attack Detected
### Web Server Logs
[Key observations from web server logs]
You can see how many logins the attacker did and the attempts to login as an admin as well as trying to search for reports on the users and their personal data.
"GET /login.php?user=admin&pass=123456 HTTP/1.1" 200 1234
"GET /assets/main.css HTTP/1.1" 200 2345
### Firewall Logs
[Key observations from firewall logs]
The amount of established statements show that there were many handshakes that occured but only after 14:32:17 and this shows that the attacker was making successful connections with others over the network.
### IDS/IPS Logs
[Key observations from IDS logs]
You can see that the attasck was detected after the attacker tried to login as an admin and it shows who was trying it as well as showing the multiple attempts of the same person logging in which would probably mean that the person is guessing in order to get into mutliple accounts to gain access to the data as theres many repeated tries.
### Database Logs
[Key observations from database logs]
It shows when the attacker first connected to the database despite the fact that he had already attempted to log in before then and ut shows how many times the attacker tried to login in order to gain data and info.
### User Access Logs
[Key observations from user access logs]
The user access logs show that the attacker had already tried to login to the admin account and was unsuccessful but was not noticed by the database or firewall yet. Once again it shwos how many times the attacker tried to log into the accounts and was unsuccessful.
## Threat Assessment
- **Attack Type**: SQL injecton
- **Attack Vector**: TCP 80 (there is unencrypted web traffic which would seem to be perfect for the attacker to sneak into)
- **Potential Impact**: Attacker could have gotten lots of data that he could use for financial gain, reputation destruction, and etc... 
- **Confidence Level**: 
High(Attacker tried to login many times and was unsuccessful and the account was an admin account which would give him a lot of information so the motive makes sense.
## Decision
- **Is this a false positive?** [Yes/No] No
- **Should this be escalated to Tier 2?** [Yes/No] Yes 
- **Reasoning**: 
The attacker was not noticed by the database on his first connection showing some errors on the company's part and had the attacker logged into the admin account peoples data would have been in grave danger
## Recommendations
[What actions should be taken next]
I believe the company should revisit their database connection detection systems as well as finding a way to block attackers out after too many missed tries
## Documentation
- **Triage Completed By**: Gabriel Lamothe
- **Date/Time**: 9/2/2026
- **Tools Used**: Logs and ids(Snort)
- **References**: Logs and google 
