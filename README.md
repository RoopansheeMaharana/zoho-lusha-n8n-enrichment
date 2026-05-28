# Zoho CRM × Lusha Enrichment × Groq AI — n8n Automation Workflow

An end-to-end automated lead enrichment and AI analysis pipeline built on n8n.
Built as a portfolio demo using dummy data to showcase the exact workflow logic
used in a real CRM automation project.

---

## What this workflow does

1. **Fetches leads** from Zoho CRM automatically every hour
2. **Enriches each lead** with phone number, job title and LinkedIn profile
   using a JavaScript-based Lusha simulation — built to mirror the exact
   Lusha API response structure
3. **Updates Zoho CRM** — writes enriched data back to each lead record automatically
4. **AI analysis** — runs every lead through Groq AI (LLaMA 3.3) for a
   priority score (1–10) and recommended next action
5. **Email report** — aggregates all results and sends one formatted
   summary to Gmail automatically after every run

---

## Workflow nodes

| Step | Node | Purpose |
|------|------|---------|
| 1 | Schedule Trigger | Runs the workflow every hour automatically |
| 2 | Zoho CRM — Get Many Leads | Fetches all leads from Zoho CRM |
| 3 | Code (JavaScript) | Lusha-style enrichment simulation — maps phone, title, LinkedIn by email |
| 4 | Zoho CRM — Update a Lead | Writes enriched data back to each lead record |
| 5 | HTTP Request (Groq API) | Sends each lead to LLaMA 3.3 for AI priority scoring |
| 6 | Aggregate | Combines all 15 lead results into one object |
| 7 | Gmail — Send a Message | Sends one formatted summary report email |

---

## Tech stack

- **n8n** — workflow automation platform
- **Zoho CRM** — lead management (OAuth2 integration)
- **Lusha API** — contact enrichment (simulated via JavaScript in this demo)
- **Groq AI / LLaMA 3.3** — lead priority scoring and recommended actions
- **Gmail** — automated report delivery

---

## How the Lusha simulation works

Since Lusha requires a paid API subscription, this demo simulates the enrichment
layer using a JavaScript lookup table inside an n8n Code node:

```javascript
const lushaData = {
  "john.smith@abctech.com": {
    phone: "+91 99999 99999",
    title: "Sales Manager",
    linkedin: "linkedin.com/in/johnsmith"
  },
  // ... 14 more leads
};

for (const item of $input.all()) {
  const email = item.json.Email?.toLowerCase();
  const enriched = lushaData[email];
  results.push({
    json: {
      ...item.json,
      Phone: enriched ? enriched.phone : item.json.Phone,
      Title: enriched ? enriched.title : "Unknown",
      LinkedIn: enriched ? enriched.linkedin : "Not found",
      Enriched: enriched ? true : false
    }
  });
}
```

In production, this Code node is replaced with an HTTP Request node
calling the real Lusha API endpoint — a 5-minute swap.

---

## How to use this workflow

1. Download the `zoho_lusha_workflow.json` file from this repo
2. Log into your n8n instance
3. Click **Import** → upload the JSON file
4. Add your credentials:
   - Zoho CRM OAuth2 (connect your Zoho account)
   - Groq API key (free at console.groq.com)
   - Gmail OAuth2 (connect your Gmail account)
5. Click **Publish** to activate

---

## Note on dummy data

This demo uses 15 dummy leads and a simulated Lusha enrichment layer
for data privacy and confidentiality purposes. The workflow logic,
node configuration, API structure, and AI prompting are identical
to a production implementation. Swapping in real credentials and
the live Lusha API is the only change needed for production use.

---

## Sample AI output

```
Lead: Sara Connor — VP Sales, NextGen Solutions
Priority Score: 9/10
Recommended Action: Schedule a product demo call within 48 hours.
High-value decision maker at a qualified account — prioritise immediately.
```

---

Built by [Roopanshee Maharana] | [roopanhsee@gmail.com]
