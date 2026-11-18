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




## Core Components

| Component | Role | Key function |
|---|---|---|
| **Splunk SIEM** | Detection and log source | Correlates endpoint logs and triggers alerts |
| **n8n** | Automation layer | Receives alerts and routes them through AI to Slack |
| **OpenAI API** | Alert triage engine | Adds context, severity, and response guidance to each alert |
| **Claude + Splunk MCP** | Investigation assistant | Turns plain-English questions into Splunk queries |
| **Slack** | Notification channel | Delivers enriched alerts to the SOC team |

### 1. Splunk SIEM
- Collects Windows and Linux security logs through Universal Forwarders
- Correlates events and triggers alerts for the simulated attacks
- Sends each triggered alert to n8n through a webhook
- Exposes its data to Claude through the Splunk MCP Server

### 2. n8n Automation
- Listens for Splunk alerts on a webhook
- Extracts the alert name and result data and formats them into a structured message
- Sends the message to OpenAI for analysis
- Forwards the AI response to Slack

### 3. OpenAI: SOC Analyst Engine
The model is prompted to act as a **Tier-1 SOC analyst**. For each alert it:

- Classifies the attack type
- Assigns a severity level
- Maps the activity to **MITRE ATT&CK** tactics and techniques
- Judges whether the alert is likely a real incident or a false positive
- Recommends triage and mitigation steps based on industry best practice

### 4. Claude + Splunk MCP
Claude Desktop connects to Splunk through the MCP Server, so analysts can hunt threats in
natural language instead of writing SPL by hand. Claude translates the question into a
Splunk query and returns the results with context.

**Example prompts**
- "Show all failed logins from this IP."
- "When did this brute-force attack begin?"
- "Which users were added in the last 24 hours?"
- "How should we mitigate this brute-force attack?"

## Agentic SOC Capabilities

- **Automated Level-1 triage**: every alert is analyzed before a human sees it
- **Attack classification**: alert type, severity, and MITRE mapping
- **Log correlation**: Splunk rules link raw events into meaningful alerts
- **Natural-language threat hunting**: ask questions instead of writing queries
- **Mitigation advice**: response recommendations with every alert
- **Slack-based reporting**: enriched alerts delivered where the team already works












Threat hunting via natural language

Mitigation advisory generation

Slack-based SOC reporting
