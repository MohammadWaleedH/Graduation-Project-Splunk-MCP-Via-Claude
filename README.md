# Agentic SOC: Splunk SIEM + Claude (MCP) + n8n/ChatGPT Automation


I designed and implemented an AI-driven security operations workflow that connects **Splunk, Claude (via MCP), OpenAI, n8n and Slack** into an AI-assisted SOC analyst. It detects simulated attacks, enriches alerts with MITRE ATT&CK context and response recommendations, notifies the team in Slack, and lets analysts investigate their SIEM data in plain English, all with a human staying in control of the final decision.

> Built as part of my thesis: ACEIS Agentic SOC Automation . This was a VM-based lab, so this repo documents the build with screenshots, sanitized configs, and results.

## Problem Statement

Traditional SOC environments struggle with:

- **High alert volume**: more alerts than analysts can realistically review
- **Manual triage overhead**: repetitive first-level analysis of every alert
- **Slow investigation time**: writing queries and correlating logs by hand
- **Analyst fatigue**: leading to missed or delayed responses

## Research Questions

This project was built to explore:

1. Can AI triage SIEM alerts and reduce the time analysts spend on first-level review?
2. Can LLMs turn raw logs into structured incident analysis?
3. Can automation reduce mean time to respond (MTTR) without removing human oversight?

## Architecture


<img width="1495" height="722" alt="Agentic SOC-Splunk MCP - Archtectural Diagram" src="https://github.com/user-attachments/assets/fee8707f-e4cd-44fc-ba5d-1f3d3adcbefc" />





**How it works**

1. **Endpoints** (Windows and Linux VMs) send security logs to Splunk through Universal Forwarders.
2. **Splunk Enterprise** correlates the logs and triggers alerts when it detects attack patterns.
3. **n8n** receives each alert through a webhook and passes it to the **OpenAI API**, which adds
   attack context, severity, and recommended incident response steps.
4. The enriched alert is delivered to **Slack** for the SOC team.
5. For deeper investigation, an analyst asks questions in **Claude Desktop**, which queries
   Splunk directly through the **Splunk MCP Server**.
















Core Components

1️⃣ Splunk SIEM

Generates real-time alerts

Provides log data for investigation

Connected to Claude via MCP for contextual queries

2️⃣ n8n Automation

Listens for Splunk alerts (via webhook or API)

Formats alert payload

Sends structured data to OpenAI

Routes AI output to Slack

3️⃣ OpenAI – SOC Analyst Engine

Processes alerts and:

Classifies attack type

Assigns severity level

Maps to MITRE ATT&CK (when applicable)

Generates:

Incident summary

Timeline analysis

Root cause hypothesis

Mitigation recommendations

4️⃣ Claude + Splunk MCP

Allows natural language threat hunting:

Example prompts:

“When did this attack begin?”

“Show all failed logins from this IP.”

“Was lateral movement detected?”

“How should we mitigate this brute-force attack?”

Claude translates prompts into structured Splunk queries and returns contextualized results.

🤖 Agentic SOC Capabilities

Automated Level-1 triage

Attack classification

Log correlation

Threat hunting via natural language

Mitigation advisory generation

Slack-based SOC reporting
