# 📧 Intelligent Email Lead Triage & Instant Alerting (n8n Template)

**Never let a hot lead sit unread in your inbox again.**

This n8n workflow watches your Gmail inbox, uses AI to instantly classify every incoming email, logs it to your CRM sheet, and sends your team a real-time Slack alert the moment a high-priority lead comes in — all without a human touching the inbox.

Built for sales teams, agencies, and small businesses that can't afford to miss a hot lead buried between spam and support tickets.

---
<img width="1104" height="826" alt="Opera Snapshot_2026-09-30_170852_localhost" src="https://github.com/user-attachments/assets/bf2aef52-8703-40ab-9317-f0a380594afc" />


## 🧠 How It Works

```
New email arrives (Gmail)
        │
        ▼
Extract & sanitize sender, subject, and body
        │
        ▼
AI classifies the email (Google Gemini):
  • Category: Hot Lead / Support Request / Billing/Invoice / Spam/Junk
  • Urgency: High / Medium / Low
  • 1-sentence summary
  • Suggested reply draft
        │
        ▼
Log entry to Google Sheets CRM
        │
        ▼
   High urgency OR Hot Lead? ──No──▶ Done, logged only
        │
       Yes
        │
        ▼
Slack alert sent instantly with summary,
sender details, and suggested reply
```

Every email gets triaged and logged automatically. Only the ones that actually matter interrupt your team.

---

## ✅ Prerequisites

| Requirement | Purpose |
|---|---|
| **n8n instance** (self-hosted or cloud) | Runs the workflow |
| **Gmail account** (OAuth2) | Triggers on new incoming email |
| **Google Gemini API key** | Classifies and summarizes each email |
| **Google Sheets** | Acts as your lightweight CRM log |
| **Slack workspace + bot** | Sends instant urgent-lead alerts |

---

## ⚙️ Setup Instructions

### 1. Fix the workflow connections (if importing raw)
Make sure `Extract & Sanitize Email Data` connects to `AI Lead Triage Analysis` (not a missing "AI Lead Routing" node), and that the `Check Urgent Lead` node's second condition checks `Category equals "Hot Lead"` rather than `Urgency equals "Hot Lead"`.

### 2. Connect Gmail
Create a Gmail OAuth2 credential (enable Gmail API in Google Cloud Console) and attach it to **Gmail Trigger - New Email**. Optionally filter by label so you're not processing your entire inbox.

### 3. Connect Google Gemini
Create an API key at Google AI Studio, add it as a "Google Gemini(PaLM) Api" credential, and attach to **Google Gemini Chat Model**.

### 4. Set up your Google Sheet
Create a sheet with columns: `Timestamp | Sender Email | Category | Urgency | Summary`. Connect your Google Sheets OAuth2 credential and select the correct spreadsheet/tab in **Log Lead to Google Sheets CRM**.

### 5. Connect Slack
Create a Slack app with `chat:write` scope, install it to your workspace, invite the bot to your target channel (e.g. `#leads-urgent`), and attach the credential to **Slack Alert - Urgent Lead**. Set the channel in the node.

### 6. Test it
Send yourself a test email with clear buying intent (e.g. "I'd like a demo of your enterprise plan") and confirm it's logged to your sheet AND triggers a Slack alert.

### 7. Activate
Toggle the workflow **Active**.

---

## ⚠️ Things to Watch

- **Gmail header shape varies**: The extraction code is defensively written to handle multiple possible Gmail API response shapes, but if you hit a `.match is not a function` error, check the raw Gmail Trigger output in JSON view and adjust the header parsing accordingly.
- **Remove debug field before production**: The extraction node includes a temporary `_debug_raw` field for troubleshooting — remove it once everything works correctly.
- **AI classification isn't perfect**: Occasionally review a sample of triaged emails to confirm the category/urgency logic matches your business's real definition of "hot."

---

## 🗺️ Ideas for Extending This

- Auto-send the AI-suggested reply as a draft (not sent) for human review
- Add a CRM integration (HubSpot, Pipedrive) instead of Google Sheets
- Route different categories to different Slack channels
- Add a weekly digest summarizing all triaged leads

---

## 📄 License
Provided as-is for self-hosted use and customization.
