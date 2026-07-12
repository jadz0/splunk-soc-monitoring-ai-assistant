# Splunk Queries

## Overview

This file contains SPL queries used in the Splunk SOC Monitoring Lab with AI Assistant.

The queries focus on Windows Security Event Logs and common SOC monitoring use cases.

## Failed Logon Events - Event ID 4625

```spl
index=* EventCode=4625
| table _time Account_Name Source_Network_Address host Failure_Reason
```

## Multiple Failed Logons by User and Source IP

```spl
index=* EventCode=4625
| stats count by Account_Name Source_Network_Address host
| where count >= 5
| sort - count
```

## Successful Logons - Event ID 4624

```spl
index=* EventCode=4624
| table _time Account_Name Source_Network_Address host Logon_Type
```

## Authentication Success vs Failure

```spl
index=* (EventCode=4624 OR EventCode=4625)
| eval Authentication_Status=case(EventCode=4624, "Success", EventCode=4625, "Failure")
| stats count by Authentication_Status
```

## New User Account Created - Event ID 4720

```spl
index=* EventCode=4720
| table _time Account_Name Target_Account_Name host
```

## User Added to Security Group - Event ID 4732

```spl
index=* EventCode=4732
| table _time Account_Name Group_Name Member_Name host
```

## Account Lockout - Event ID 4740

```spl
index=* EventCode=4740
| table _time Account_Name Caller_Computer_Name host
```

## SOC AI Assistant Test Alert Search

This query creates sample alert data to test the n8n SOC AI Assistant workflow.

```spl
| makeresults
| eval alert_name="Multiple Failed Login Attempts"
| eval event_code="4625"
| eval user="john"
| eval source_ip="192.168.1.50"
| eval host="WIN-ENDPOINT-01"
| eval count="8"
| eval time=strftime(now(), "%Y-%m-%d %H:%M:%S")
| table alert_name event_code user source_ip host count time
```

## Real Failed Login Alert for n8n Workflow

This query can be used to send failed login alert data to the n8n workflow.

```spl
index=* EventCode=4625
| stats count max(_time) as last_time values(host) as host by Account_Name Source_Network_Address
| where count >= 5
| eval alert_name="Multiple Failed Login Attempts"
| eval event_code="4625"
| eval user=Account_Name
| eval source_ip=Source_Network_Address
| eval time=strftime(last_time, "%Y-%m-%d %H:%M:%S")
| table alert_name event_code user source_ip host count time
```

## Suggested Alert Settings

```text
Alert Type: Scheduled
Schedule: Every 5 minutes
Trigger Condition: Number of Results > 0
Trigger Mode: Once
Throttle: 30 minutes
Action: Webhook
Destination: n8n Webhook URL
```

## Notes

The queries may need small changes depending on the Splunk index name, field names, and Windows log format.
