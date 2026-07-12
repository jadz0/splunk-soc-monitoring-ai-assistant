# Detections

## Overview

This document explains the main detections used in the Splunk SOC Monitoring Lab with AI Assistant.

The detections are based on Windows Security Event IDs and are designed to simulate common SOC monitoring use cases.

## Event ID 4625 - Failed Logon

### Description

Windows Event ID 4625 is generated when a logon attempt fails.

### SOC Use Case

Multiple failed logons from the same source IP or against the same user account may indicate:

- Password guessing
- Brute-force activity
- Credential stuffing
- Misconfigured services
- User error

### MITRE ATT&CK Mapping

| Tactic | Technique |
|---|---|
| Credential Access | T1110 - Brute Force |

### Example SPL

```spl
index=* EventCode=4625
| stats count by Account_Name Source_Network_Address host
| where count >= 5
```

### Analyst Notes

The analyst should review:

- Source IP
- Target username
- Number of failed attempts
- Time range
- Whether a successful login occurred after the failed attempts

## Event ID 4624 - Successful Logon

### Description

Windows Event ID 4624 is generated when a user successfully logs on.

### SOC Use Case

Successful logons are usually normal, but they can be suspicious if they occur after repeated failed logons or from unusual sources.

### MITRE ATT&CK Mapping

| Tactic | Technique |
|---|---|
| Defense Evasion / Persistence / Privilege Escalation / Initial Access | T1078 - Valid Accounts |

### Example SPL

```spl
index=* EventCode=4624
| table _time Account_Name Source_Network_Address host Logon_Type
```

### Analyst Notes

The analyst should check:

- Whether the login time is expected
- Whether the source IP is known
- Whether the login followed suspicious failed attempts
- Whether the account is privileged

## Event ID 4720 - New User Account Created

### Description

Windows Event ID 4720 is generated when a new user account is created.

### SOC Use Case

New account creation may be legitimate administrative activity, but it can also indicate persistence if unauthorized.

### MITRE ATT&CK Mapping

| Tactic | Technique |
|---|---|
| Persistence | T1136 - Create Account |

### Example SPL

```spl
index=* EventCode=4720
| table _time Account_Name Target_Account_Name host
```

### Analyst Notes

The analyst should verify:

- Who created the account
- Whether the account creation was approved
- Whether the new account was added to privileged groups

## Event ID 4732 - User Added to Security Group

### Description

Windows Event ID 4732 is generated when a user is added to a local security group.

### SOC Use Case

Adding a user to a privileged group may indicate privilege escalation or account manipulation.

### MITRE ATT&CK Mapping

| Tactic | Technique |
|---|---|
| Persistence / Privilege Escalation | T1098 - Account Manipulation |

### Example SPL

```spl
index=* EventCode=4732
| table _time Account_Name Group_Name Member_Name host
```

### Analyst Notes

The analyst should review:

- Which user was added
- Which group was modified
- Who performed the change
- Whether the change was approved

## Event ID 4740 - Account Lockout

### Description

Windows Event ID 4740 is generated when a user account is locked out.

### SOC Use Case

Account lockouts can occur after repeated failed logon attempts.

### MITRE ATT&CK Mapping

| Tactic | Technique |
|---|---|
| Credential Access | T1110 - Brute Force |

### Example SPL

```spl
index=* EventCode=4740
| table _time Account_Name Caller_Computer_Name host
```

### Analyst Notes

The analyst should review failed login events before the lockout and identify the source of the attempts.

## Detection Summary

| Event ID | Detection | MITRE Technique |
|---|---|---|
| 4625 | Failed logon | T1110 - Brute Force |
| 4624 | Successful logon | T1078 - Valid Accounts |
| 4720 | New user account created | T1136 - Create Account |
| 4732 | User added to security group | T1098 - Account Manipulation |
| 4740 | Account lockout | T1110 - Brute Force |
