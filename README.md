# SOC Tier 1 Simulation Exercise

A self-contained, offline triage exercise for students learning to work a SOC alertfrom first page to escalation decision. Students receive five correlated logsources and one IDS alert, then determine whether they are looking at a real attackor a false positive — and document the call the way a Tier 1 analyst would.

Everything here is plain text. No internet, no cloud services, no special toolingrequired: a text editor and basic command-line search (`grep`, `findstr`) areenough.

## Scenario

You are a Tier 1 SOC analyst monitoring security alerts for the corporate network.At 14:32 your IDS pages you about possible SQL injection against an internal webserver. Your job is to triage the alert: confirm whether it is genuine, scope whatactually happened, and decide whether it goes up to Tier 2.

The catch — and the skill this exercise is really testing — is that no single logtells the whole story. You will have to pivot on the attacker and correlate acrosssources to see the full picture.

### Triggering Alert

    [ALERT] IDS/IPS — Rule 1000001
    Category:        SQL Injection
    Severity:        High
    Source IP:       192.168.1.105
    Destination IP:  10.10.1.25  (web01.corp.company.com)
    First seen:      2024-01-15 14:32:17
    Endpoint:        /login.php
    Status:          Open — awaiting Tier 1 triage

Note what the alert tells you and what it doesn't. The IDS gives you a startingpoint, not a complete timeline. Treat its first-seen time as a lead, not as themoment the attack began.

## Files Included

| File | Purpose |
| --- | --- |
| `README.md` | This file — scenario brief and instructions |
| `web_server_logs.txt` | Apache-style web server access logs |
| `firewall_logs.txt` | Firewall connection records |
| `ids_logs.txt` | IDS/IPS detection alerts |
| `database_logs.txt` | Database connection and query logs |
| `user_access_logs.txt` | Authentication / login-attempt logs |
| `triage_report.md` | Blank template for documenting your findings |

## Exercise Structure

### Phase 1 — Initial Assessment (30 minutes)

* Read the alert and confirm what is being claimed
* Identify the attacker and the target
* Recognize the attack pattern

### Phase 2 — Investigation (60 minutes)

* Work each log source individually
* Pivot on the attacker's IP across all five sources
* Establish the true start and end of the activity
* Determine whether the attack reached the application and the database

### Phase 3 — Documentation and Reporting (30 minutes)

* Complete `triage_report.md`
* Reach a verdict and an escalation decision
* Record your reasoning so a Tier 2 analyst could pick it up cold

## Learning Objectives

By completing this exercise, students will be able to:

* Triage an alert from a single source and avoid taking it at face value
* Perform basic log analysis across multiple, differently-formatted sources
* Correlate events by attacker, timestamp, and indicator to scope an incident
* Distinguish a real attack from benign or automated noise
* Make and justify an escalation decision
* Produce a clear, structured triage report

## Getting Started

1. Read the triggering alert above and the scenario brief.
2. Open each log file and skim it before analyzing — get a feel for its format.
3. Pick the attacker's IP out of the alert and trace it through every log.
4. Pay attention to timestamps. Where does the activity actually start? Where doesit end? Do all five sources agree?
5. Record findings as you go in `triage_report.md`.
6. Decide: real incident or false positive — and does it escalate to Tier 2?

## Expected Outcomes

A complete submission identifies the activity as SQL injection from a single host,preceded by a short burst of credential guessing, and shows that the attacker'srequests reached the database while no login actually succeeded. Strong submissionsgo further: they notice that the IDS, firewall, and database logs captured only thetail end of the activity, and that the full scope is visible only by correlating theweb server and authentication logs. The expected decision is to escalate — theinjection is confirmed and reached the data layer, even though no account wascompromised.

## Scoring Rubric

| Category | Score | Description |
| --- | --- | --- |
| Initial Assessment | 20  | Correct identification of attack type and source |
| Log Analysis | 30  | Proper correlation of logs from different sources |
| Threat Evaluation | 25  | Accurate assessment of threat level and impact |
| Documentation | 20  | Clear, structured triage report |
| Communication | 5   | Sound escalation decision |

## Post-Exercise Discussion Questions

* How would you tell a legitimate vulnerability scan apart from a real attack?
* Which additional logs or data sources would have helped you scope this faster?
* What do you do when you are genuinely unsure of the threat level — escalate, hold,or investigate further?
* How would you automate the repetitive parts of this triage?
* How would you summarize this incident for management in two sentences?

## Repository Structure

    Mini-SOC01/
    ├── README.md              # Scenario brief and student instructions (this file)
    ├── web_server_logs.txt    # Apache-style web access logs — full attack window
    ├── firewall_logs.txt      # Firewall connection records — partial window
    ├── ids_logs.txt           # IDS/IPS SQL injection alerts — partial window
    ├── database_logs.txt      # DB connection + executed queries — partial window
    ├── user_access_logs.txt   # Login attempts (attacker + legitimate users)
    ├── triage_report.md       # Blank student template
    └── solution.md            # Instructor answer key — see note below

> **Instructor note:** `solution.md` is the facilitator answer key. Remove it or moveit to a private location before distributing this exercise to students. It alsocontains grading guidance tied to the rubric above.

## License / Use

Built as a classroom exercise. Free to use and adapt for educational purposes.Attribution appreciated.
