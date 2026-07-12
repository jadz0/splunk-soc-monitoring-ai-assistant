# Setup Guide

## Overview

This guide explains the main setup steps used in the Splunk SOC Monitoring Lab with AI Assistant.

The lab consists of:

- Splunk Enterprise running on Ubuntu
- Windows endpoint with Splunk Universal Forwarder
- Windows Security Logs forwarded to Splunk
- Splunk dashboards and alerts
- n8n workflow for AI-assisted SOC triage
- Discord webhook for alert delivery

## 1. Install Splunk Enterprise on Ubuntu

Splunk Enterprise was installed on an Ubuntu virtual machine.

After installation, Splunk was started and accessed through the web interface.

Default Splunk web port:

```text
8000
```

Default receiving port for forwarders:

```text
9997
```

## 2. Enable Receiving in Splunk

In Splunk Enterprise:

```text
Settings → Forwarding and Receiving → Receive data → New Receiving Port
```

Configured receiving port:

```text
9997
```

This allows Splunk to receive logs from the Windows endpoint.

## 3. Install Splunk Universal Forwarder on Windows

The Splunk Universal Forwarder was installed on a Windows virtual machine.

During setup, the forwarder was configured to send data to the Splunk Enterprise server.

Example destination:

```text
<SPLUNK_SERVER_IP>:9997
```

## 4. Forward Windows Security Logs

The Windows endpoint was configured to forward Windows Security Event Logs to Splunk.

The main log source used in this project:

```text
WinEventLog:Security
```

## 5. Verify Logs in Splunk

In Splunk Search & Reporting, Windows Security Logs were verified using searches such as:

```spl
index=* source="WinEventLog:Security"
```

or:

```spl
index=* EventCode=4625
```

## 6. Create Security Dashboard

A Splunk dashboard was created to monitor:

- Failed logons
- Successful logons
- Account creation
- Security group changes
- Account lockouts
- Authentication success vs failure

## 7. Create Saved Searches and Alerts

Saved searches were created for common SOC use cases, such as multiple failed login attempts.

Example:

```spl
index=* EventCode=4625
| stats count by Account_Name Source_Network_Address host
| where count >= 5
```

## 8. Configure n8n SOC AI Assistant

An n8n workflow was created with the following flow:

```text
Webhook → Edit Fields → Basic LLM Chain → Discord Webhook
```

The workflow receives alert data, cleans the fields, sends the data to an AI model, and posts the final report to Discord.

## 9. Configure Discord Webhook

A Discord channel was created for SOC alerts.

A Discord webhook was created from the channel settings and used inside n8n to send AI-generated SOC reports.

## 10. Testing

The workflow was tested using sample alert data before connecting it to Splunk alert actions.

Test alert fields included:

- Alert name
- Event code
- User
- Source IP
- Host
- Event count
- Time

## Important Security Notes

Do not upload sensitive values to GitHub, including:

- Splunk passwords
- OpenAI API keys
- Discord webhook URLs
- n8n webhook URLs
- Private credentials
