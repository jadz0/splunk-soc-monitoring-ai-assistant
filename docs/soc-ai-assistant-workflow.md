# SOC AI Assistant Workflow

## Overview

This document explains the SOC AI Assistant upgrade added to the Splunk SOC Monitoring Lab.

The workflow uses n8n to receive Splunk alert data, process it with an AI model, and send a structured SOC triage report to a Discord alert channel.

This was built as a lab proof-of-concept to demonstrate how AI-assisted automation can support alert triage and documentation.

## Workflow

```text
Splunk Alert Data
    ↓
n8n Webhook
    ↓
Edit Fields
    ↓
Basic LLM Chain
    ↓
Discord Webhook
```

## Purpose

The purpose of the workflow is to help organize alert information into a clear SOC-style report.

The AI-generated report includes:

- Alert summary
- IOC details
- Severity assessment
- MITRE ATT&CK mapping
- Recommended actions
- Escalation guidance

## n8n Nodes Used

### Webhook

The Webhook node receives alert data.

In the lab, the workflow was tested using sample alert data that simulates a Splunk alert payload.

Example fields:

- alert_name
- event_code
- user
- source_ip
- host
- count
- time

### Edit Fields

The Edit Fields node cleans and organizes the incoming alert data before it is sent to the AI model.

This makes the workflow easier to manage and improves the quality of the final SOC report.

### Basic LLM Chain

The Basic LLM Chain sends the cleaned alert data to an AI model with a SOC analyst prompt.

The prompt tells the AI to generate a structured Discord-ready SOC alert report.

### Discord Webhook

The Discord webhook sends the AI-generated report to a Discord SOC alert channel.

This simulates how a SOC team could receive structured alert summaries in a communication channel.

## Sample Alert

```json
{
  "result": {
    "alert_name": "Multiple Failed Login Attempts",
    "event_code": "4625",
    "user": "john",
    "source_ip": "192.168.1.50",
    "host": "WIN-ENDPOINT-01",
    "count": "8",
    "time": "2026-07-12 21:35:00"
  },
  "results_link": "Test alert from lab workflow"
}
```

## Example Output

```text
🚨 SOC ALERT REPORT

Summary:
- Alert: Multiple Failed Login Attempts
- User: john
- Host: WIN-ENDPOINT-01
- Source IP: 192.168.1.50
- Event Count: 8
- Time: 2026-07-12 21:35:00

IOC Enrichment:
- IP: 192.168.1.50
- Type: Private/Internal
- Notes: Internal IP address. Correlate with asset inventory and authentication logs.

Severity Assessment:
- MITRE Tactic: Credential Access
- Technique: T1110 - Brute Force
- Severity: Medium
- Reason: Multiple failed logon attempts against the same user may indicate password guessing or brute-force activity.

Recommended Actions:
1. Review failed and successful logon events for the same user.
2. Confirm whether the source IP is expected.
3. Escalate if the activity is suspicious or followed by successful login activity.

Final Summary:
Multiple failed login attempts were detected and should be reviewed for possible brute-force activity.
```

## Security Notes

The following information should not be uploaded to GitHub:

- OpenAI API keys
- Discord webhook URLs
- n8n webhook URLs
- Splunk credentials
- Private passwords or tokens

Webhook URLs and API keys should be blurred in screenshots.

## Limitations

The AI assistant does not replace analyst judgment.

It provides a structured summary based on the alert data provided. Analysts must still validate the activity, review logs, check business context, and decide whether escalation is required.

## Skills Demonstrated

- n8n workflow automation
- Webhook-based alert handling
- AI-assisted SOC reporting
- MITRE ATT&CK mapping
- Discord alert delivery
- Alert triage documentation
