# zoho-lusha-n8n-enrichment
# Zoho CRM × Lusha-style Enrichment × AI Analysis — n8n Workflow

An automated lead enrichment and AI analysis pipeline built on n8n.

## What this does
- Fetches leads from Zoho CRM every hour automatically
- Enriches each lead with phone, job title and LinkedIn (Lusha-style)
- Runs every lead through Groq AI (LLaMA) for priority scoring
- Sends a formatted summary report to Gmail automatically

## Workflow nodes
1. Schedule Trigger — runs every hour
2. Zoho CRM — fetches all leads
3. Code node — contact data enrichment (simulates Lusha API)
4. Zoho CRM — updates lead records with enriched data
5. HTTP Request — Groq AI analysis per lead
6. Aggregate — combines all results
7. Gmail — sends one summary report email

## Tech stack
- n8n (workflow automation)
- Zoho CRM (lead management)
- Lusha API (contact enrichment — simulated in this demo)
- Groq AI / LLaMA 3.3 (lead analysis)
- Gmail (automated reporting)

## Note on dummy data
This demo uses dummy leads and a simulated Lusha enrichment layer
for confidentiality purposes. The workflow logic is identical to
a production implementation — swapping in real API credentials
is the only change needed.

## How to use
1. Import the `.json` file into your n8n instance
2. Add your Zoho CRM OAuth credentials
3. Add your Groq API key
4. Add your Gmail credentials
5. Activate the workflow
