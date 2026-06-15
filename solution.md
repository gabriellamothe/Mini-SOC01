[# SOC Tier 1 Triage Report - SOLUTION

## Incident Details
- **Alert ID**: 123456789-123456798
- **Timestamp**: 2024-01-15 14:32:17 - 14:32:26
- **Source IP**: 192.168.1.105
- **Destination IP**: 10.10.1.25 (web01.corp.company.com)
- **Severity**: High

## Summary of Findings
Multiple SQL injection attempts detected from IP 192.168.1.105 targeting /login.php over a 10-second period. All requests returned HTTP 200 status codes and no successful authentication occurred. Attack appears automated but web server shows no evidence of compromise.

## Evidence Collected

### Web Server Logs
- 10 suspicious requests from 192.168.1.105 to `/login.php` with SQLi payloads:
  - `user=1' OR '1'='1' --`
  - `user=1' OR 1=1 --` 
  - `user=admin'--`
- All requests returned `200 1234` (success with consistent 1234 bytes)
- 50+ legitimate requests from other IPs mixed throughout timeline

### Firewall Logs
- Continuous TCP/80 connections from 192.168.1.105 to 10.10.1.25 marked `ESTABLISHED`
- No dropped/blocked connections during attack window
- Normal traffic flow maintained

### IDS/IPS Logs
- 10 alerts triggered (IDs 123456789-123456798) on Rule 1000001
- Consistent payloads detected: `1' OR 1=1 --`, `1' OR '1'='1' --`, `admin'--`
- High severity SQL injection category confirmed

### Database Logs
- Malicious queries reached database layer:
  - `SELECT * FROM users WHERE username='1' OR 1=1 --'`
  - `SELECT * FROM users WHERE username='admin'--'`
- No evidence of data exfiltration or unauthorized access

### User Access Logs
- 10 failed login attempts from 192.168.1.105 matching web server timestamps
- SQLi usernames (`1' OR '1'='1' --`, `admin'--`) all resulted in `Status: Failed`
- 50 legitimate successful logins from other internal IPs during same timeframe

## Threat Assessment
- **Attack Type**: SQL Injection (automated scanner)
- **Attack Vector**: GET parameter injection via `/login.php?user=`
- **Potential Impact**: Low - no successful exploitation, all requests returned identical responses
- **Confidence Level**: High (95%) - clear attack pattern across all log sources

## Decision
- **Is this a false positive?** No - genuine SQLi attempt detected
- **Should this be escalated to Tier 2?** Yes
- **Reasoning**: 
  - Confirmed malicious SQLi payloads across web, IDS, database, and auth logs
  - Rapid automated scanning pattern (10 attempts in 10 seconds)
  - Requires IP blocking, WAF rule tuning, and application vulnerability assessment

## Recommendations
1. **Immediate**: Block IP 192.168.1.105 at firewall perimeter
2. **Short-term**: Tune IDS rule 1000001 to reduce noise on benign traffic
3. **Medium-term**: Implement WAF rules for SQLi protection on /login.php
4. **Long-term**: Code review of login.php for parameterized queries
5. **Monitoring**: Watch for repeat attempts from 192.168.1.105 or similar patterns

## Documentation
- **Triage Completed By**: Tier 1 Analyst
- **Date/Time**: 2024-01-15 15:00:00
- **Tools Used**: Native log file analysis, timestamp correlation
- **References**: 
  - IDS Rule 1000001 documentation
  - OWASP SQL Injection cheat sheet
  - Company incident response playbook
](https://github.com/jdelvalle10/Mini-SOC01
)<img width="729" height="80" alt="image" src="https://github.com/user-attachments/assets/a8f5c53c-f8df-45df-b649-b82e2011f956" />
