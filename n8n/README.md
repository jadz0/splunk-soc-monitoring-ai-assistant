# n8n SOC AI Assistant

## Overview

This folder contains documentation and sample files for the n8n SOC AI Assistant workflow used in the Splunk SOC Monitoring Lab with AI Assistant.

The workflow receives alert data, sends it to an AI model, and posts a structured SOC triage report to a Discord alert channel.

## Workflow

```text
Webhook
    ↓
Edit Fields
    ↓
Basic LLM Chain
    ↓
Discord Webhook
```

## Files

| File | Description |
|---|---|
| ai-agent-prompt.txt | Prompt used in the AI node |
| sample-alert-payload.json | Sample alert payload used for testing |

## Alert Fields

The workflow expects alert data with these fields:

- alert_name
- event_code
- user
- source_ip
- host
- count
- time

## Output

The AI model generates a Discord-ready SOC alert report that includes:

- Summary
- IOC enrichment
- Severity assessment
- MITRE ATT&CK mapping
- Recommended actions
- Final summary

## Security Notes

Do not upload real webhook URLs, API keys, or credentials to GitHub.

Blur sensitive information in screenshots before publishing.