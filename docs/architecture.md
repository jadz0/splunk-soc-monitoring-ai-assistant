# Architecture

## Overview

This lab simulates a basic SOC monitoring environment using Splunk Enterprise, Windows Security Logs, and an AI-assisted alert triage workflow.

A Windows endpoint forwards security events to a Splunk Enterprise server running on Ubuntu. Splunk is used to search, analyze, visualize, and alert on security events. The project was later upgraded with an n8n SOC AI Assistant workflow that sends alert data to Discord as a structured triage report.

## Lab Components

### Windows Endpoint

The Windows virtual machine generates security events such as:

- Successful logons
- Failed logons
- Account lockouts
- New user account creation
- Group membership changes

These events are collected from the Windows Security Event Log.

### Splunk Universal Forwarder

The Splunk Universal Forwarder is installed on the Windows endpoint.

Its role is to forward Windows Security Logs to the Splunk Enterprise server.

### Splunk Enterprise Server

Splunk Enterprise is installed on an Ubuntu virtual machine.

Splunk receives logs from the Windows endpoint, indexes them, and allows the analyst to search and investigate security events using SPL.

### Security Monitoring Dashboard

A custom Splunk dashboard was created to monitor important Windows security events.

The dashboard includes panels for failed logins, successful logins, account creation, group changes, and account lockouts.

### n8n SOC AI Assistant

The project includes an n8n workflow that receives Splunk alert data through a webhook.

The alert data is processed by an AI model, which generates a SOC-style triage report. The report is then sent to a Discord SOC alert channel.

## Data Flow

```text
Windows Endpoint
    ↓
Windows Security Logs
    ↓
Splunk Universal Forwarder
    ↓
Splunk Enterprise on Ubuntu
    ↓
Splunk Searches / Dashboards / Alerts
    ↓
n8n Webhook
    ↓
AI SOC Triage Report
    ↓
Discord SOC Alert Channel
```

## Security Use Cases

This lab focuses on common SOC monitoring use cases:

- Detecting repeated failed logins
- Reviewing successful logins
- Monitoring new user account creation
- Detecting users added to security groups
- Identifying account lockouts
- Generating AI-assisted SOC triage reports

## Notes

This is a lab environment and not a production SOC deployment. The purpose is to demonstrate practical SOC skills, SIEM monitoring, alert investigation, and automation.
