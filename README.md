

# 📬 AI-Powered Email Automation & Lead Intelligence System

> Built for **BerlinBridge Consultancy GmbH** — an operations & hospitality consulting firm based in Germany.

![n8n](https://img.shields.io/badge/n8n-Workflow_Automation-orange?style=for-the-badge&logo=n8n)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o-412991?style=for-the-badge&logo=openai)
![Microsoft Outlook](https://img.shields.io/badge/Microsoft-Outlook-0078D4?style=for-the-badge&logo=microsoft-outlook)
![HubSpot](https://img.shields.io/badge/HubSpot-CRM-FF7A59?style=for-the-badge&logo=hubspot)
![Google Sheets](https://img.shields.io/badge/Google-Sheets-34A853?style=for-the-badge&logo=google-sheets)

---

## 📌 Problem Statement

BerlinBridge Consultancy receives **dozens of inbound emails daily** across multiple departments — Sales, Admin, Accounting, COO, CEO, and Customer Care. The existing process was:

- ❌ Fully manual — staff read, categorized, and replied to every email
- ❌ No lead scoring or prioritization system
- ❌ Hot leads were missed or delayed due to inbox overload
- ❌ No CRM sync — contact data was scattered across inboxes
- ❌ No audit trail of who contacted the company or when
- ❌ Replies were inconsistent across departments and team members

**The result:** slow response times, lost opportunities, and inconsistent client experience.

### ✅ Solution

An intelligent **n8n workflow** that reads every inbound email, understands its intent using GPT-4o, routes it to the correct department, generates a professional AI reply, scores the lead, syncs to HubSpot CRM, and logs everything to Google Sheets — **completely automatically, 24/7**.

---

## 🏗️ System Architecture Overview

```
Microsoft Outlook (Trigger)
        │
        ▼
[Batch Split & Date Filter]
        │
        ▼
[Self-Email Filter] ──→ STOP (if self-sent)
        │
        ▼
[Booking Email Check] ──→ STOP (if calendar invite)
        │
        ▼
[Detect Reply vs New Email]
        │
   ┌────┴─────┐
   ▼          ▼
Reply       New Email
   └────┬─────┘
        ▼
[Human Request Detection]
        │
   ┌────┴──────┐
   ▼           ▼
Human       AI Handles
Alert         │
              ▼
        [Edit Fields + Normalize]
              │
              ▼
        [AI Summary — GPT-4o]
              │
              ▼
        [AI Department Routing — GPT-4o]
              │
    ┌─────────┼──────────────────────┐
    ▼         ▼          ▼     ▼     ▼     ▼
  Admin  Accounting  COO  CEO  Sales  Customer Care
    │         │          │     │      │        │
    └─────────┴──────────┴─────┘      │        │
         [Staff Notified]             │        │
         [Folder Moved]               ▼        │
         [Marked Read]         [Lead Scoring]  │
                                      │        │
                               [HubSpot CRM]   │
                               [Client Email]  │
                               [Sheets Log]    │
                                               ▼
                                       [Customer Reply]
```

---

## 🛠️ Tech Stack & Tools

| Tool | Purpose |
|---|---|
| **n8n** | Workflow orchestration engine |
| **Microsoft Outlook** | Email trigger, reading, sending, folder management |
| **OpenAI GPT-4o** | AI summary, department routing, reply drafting |
| **HubSpot CRM** | Contact creation, deal tracking, pipeline management |
| **Google Sheets** | Audit logging, sales tracking, follow-up scheduling |

---

## ✨ Features

### 📥 Smart Email Ingestion
- Polls inbox **every minute** for new unread emails
- Filters out self-sent emails automatically
- Filters out calendar/booking notifications
- Detects whether an email is a **new message** or a **reply thread**
- Batch processes multiple emails safely (1 per item to prevent data mixing)

### 🧠 AI-Powered Understanding (GPT-4o)
- Reads and summarizes every email in 1–2 sentences
- Understands full reply thread context
- Routes to the correct department with high accuracy
- Default routing fallback is always **Sales**

### 🏢 Department Routing Engine

Six departments with dedicated AI agents:

| Department | Trigger |
|---|---|
| **Sales** *(default)* | Pricing, demos, interest, general inquiries |
| **Admin** | HR, internal office, access requests |
| **Accounting** | Invoices, payments, billing, refunds |
| **COO** | Operations, process improvements |
| **CEO** | Strategy, investment, board-level |
| **Customer Care** | Existing customer issues, complaints |

Each department gets:
- ✅ AI-generated professional email reply
- ✅ Internal staff notification email
- ✅ Email moved to department Outlook folder
- ✅ Message marked as read automatically

### 🔥 Lead Scoring Engine (Sales Branch)

Keyword-based scoring system (0–100):

| Intent Level | Keywords | Score |
|---|---|---|
| 🔥 High | buy, pricing, cost, get started, subscribe | +50 |
| 🎯 Demo | demo, trial, interested, walkthrough | +40 |
| ⚡ Mid | details, explain, how, what | +15 |
| 🏢 Service | service, solution, consultation | +20 |
| ❄️ Low | just checking, hi, hello | −5 |

**Lead Classification:**
- Score ≥ 70 → 🔥 **Hot Lead** — immediate alert sent to sales team
- Score ≥ 40 → 🌡️ **Warm Lead**
- Score < 40 → ❄️ **Cold Lead**

### 🤝 HubSpot CRM Integration

For every qualified Sales lead:
1. **Contact created or updated** — name, email, phone, lead score, AI summary
2. **Deal searched** — if exists → updated; if not → new deal created
3. **Pipeline stage** set automatically (`qualifiedtobuy` or `appointmentscheduled`)
4. All linked to the assigned deal owner

### 📊 Google Sheets Logging

Three sheets maintained automatically:

| Sheet | Columns |
|---|---|
| `Sheet1` (General Log) | Message ID, Sent To, Status, Time, User Message |
| `Sales` | Name, Email, Lead Score, Message ID, Created At |
| `follow_up` | Name, Email, M_Id, 1st/2nd/3rd Follow-Up Dates |

---

## 📁 Repository Structure

```
📦 email-automation-n8n/
├── 📄 README.md                          ← You are here
├── 📄 email_automation_sanitized.json    ← n8n workflow (import this)
└── 📄 .gitignore
```

---

## 🚀 Setup & Installation Guide

### Prerequisites

Before you begin, ensure you have:

- [ ] **n8n** installed (self-hosted or cloud at [n8n.io](https://n8n.io))
- [ ] **Microsoft 365** account with Outlook access
- [ ] **OpenAI API** account with GPT-4o access
- [ ] **HubSpot** account (Free CRM works)
- [ ] **Google** account with a Google Sheet ready

---

### Step 1 — Import the Workflow

1. Open your **n8n dashboard**
2. Go to **Workflows → Import from File**
3. Upload `email_automation_sanitized.json`
4. The workflow loads with all nodes — **do not activate yet**

---

### Step 2 — Microsoft Outlook Credential

1. Go to [Azure Portal](https://portal.azure.com) → **Azure Active Directory → App Registrations → New Registration**
2. Add API permissions:
   - `Mail.Read`
   - `Mail.ReadWrite`
   - `Mail.Send`
3. Copy **Client ID** and **Client Secret**
4. In n8n → **Settings → Credentials → New → Microsoft Outlook OAuth2**
5. Paste credentials and authorize
6. Assign this credential to **every Outlook node** in the workflow

---

### Step 3 — OpenAI Credential

1. Go to [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. Create a new **API Key**
3. In n8n → **Credentials → New → OpenAI**
4. Paste your API key and save
5. Assign to all **OpenAI Chat Model** nodes in the workflow

> ⚠️ Ensure your OpenAI plan has access to **gpt-4o**.

---

### Step 4 — Google Sheets Credential

1. In n8n → **Credentials → New → Google Sheets OAuth2**
2. Authorize via Google
3. Create a Google Sheet with these **3 tabs**:

**Tab 1 — `Sheet1`**
```
Message ID | Sent TO | Status | Time | User_Message
```

**Tab 2 — `Sales`**
```
Name | Email | Lead score | M_Id | Created_At
```

**Tab 3 — `follow_up`**
```
M_Id | Name | Email | 1st_msg_date | 1st_followUp | 2nd_followUp | 3rd_followUp
```

4. Copy your **Sheet ID** from the URL:
```
https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID_HERE/edit
```
5. Replace `YOUR_GOOGLE_SHEET_ID` in all Google Sheets nodes

---

### Step 5 — HubSpot Integration

1. Go to [HubSpot Developer Portal](https://developers.hubspot.com/)
2. **Settings → Integrations → Private Apps → Create a Private App**
3. Enable these scopes:
   - `crm.objects.contacts.write`
   - `crm.objects.deals.write`
   - `crm.objects.deals.read`
4. Copy the **Access Token**
5. In n8n → **Credentials → New → HubSpot App Token** → paste and save
6. Assign to all **HubSpot nodes** in the workflow
7. Find your **HubSpot Deal Owner ID**:
   - HubSpot → Settings → Users → click your profile → copy ID from URL
   - Replace `YOUR_HUBSPOT_OWNER_ID` in the **Create a deal** node

---

### Step 6 — Outlook Folder IDs

Create folders in Outlook for each department: `Admin`, `Accounting`, `COO`, `CEO`, `Customer Care`, `Sales`

Find each folder ID using [Microsoft Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer):

```
GET https://graph.microsoft.com/v1.0/me/mailFolders
```

Replace these in the **Move to Folder** nodes:

| Placeholder | Description |
|---|---|
| `YOUR_OUTLOOK_INBOX_FOLDER_ID` | Your monitored inbox folder ID |
| `YOUR_OUTLOOK_FOLDER_ID_ADMIN` | Admin folder ID |
| `YOUR_OUTLOOK_FOLDER_ID_ACCOUNTING` | Accounting folder ID |
| `YOUR_OUTLOOK_FOLDER_ID_COO` | COO folder ID |
| `YOUR_OUTLOOK_FOLDER_ID_CEO` | CEO folder ID |
| `YOUR_OUTLOOK_FOLDER_ID_CUSTOMER_CARE` | Customer Care folder ID |
| `YOUR_OUTLOOK_FOLDER_ID_SALES` | Sales folder ID |

---

### Step 7 — Company Emails & Name

Replace all placeholders across the workflow:

| Placeholder | Replace With |
|---|---|
| `YOUR_ASSISTANT_EMAIL@YOUR_DOMAIN.com` | e.g. `assistant@yourcompany.com` |
| `admin@YOUR_DOMAIN.com` | e.g. `admin@yourcompany.com` |
| `accounting@YOUR_DOMAIN.com` | e.g. `accounting@yourcompany.com` |
| `coo@YOUR_DOMAIN.com` | e.g. `coo@yourcompany.com` |
| `ceo@YOUR_DOMAIN.com` | e.g. `ceo@yourcompany.com` |
| `info@YOUR_DOMAIN.com` | e.g. `info@yourcompany.com` |
| `sales@YOUR_DOMAIN.com` | e.g. `sales@yourcompany.com` |
| `YOUR_COMPANY_NAME` | Your actual company name |

---

### Step 8 — Activate

1. Double-check all credentials are assigned (no red warning icons)
2. Click **Save**
3. Toggle the workflow **Active**
4. Send a test email to your monitored inbox
5. Monitor live in the **Executions** tab

---

## 🔄 End-to-End Flow Summary

```
📧 Email arrives in Outlook
     ↓ Filtered (self / calendar skipped)
     ↓ Detected as New or Reply
     ↓ Human agent check
     ↓ AI summarizes (GPT-4o)
     ↓ AI routes to department (GPT-4o)
     ↓ AI reply generated
     ↓ Staff notified via email
     ↓ Outlook: Marked read + moved to folder
     ↓ [Sales only] Lead scored 0–100
     ↓ Hot Lead? → Immediate alert
     ↓ HubSpot: Contact + Deal synced
     ↓ Client email sent
     ↓ Google Sheets logged
```

---

## ⚙️ Customization

### Change Lead Score Thresholds
In the **Lead Scoring1** Code node:
```javascript
if (score >= 70) lead_type = "Hot";       // adjust hot threshold
else if (score >= 40) lead_type = "Warm"; // adjust warm threshold
```

### Add a New Department
1. Add a condition in **Department Routing** (Switch node)
2. Create a new **Chain LLM** node (copy existing, update prompt)
3. Add staff notification email node
4. Add **Move to Folder** node with the new folder ID
5. Connect in sequence

### Modify AI Reply Tone
Each department's Chain LLM node has a `text` prompt field. Edit the instructions, tone, and signature directly inside each node.

---

## 🔐 Security Best Practices

- ✅ Never commit the original JSON with real credentials
- ✅ Always use the sanitized version for version control
- ✅ Store sensitive values in n8n environment variables where possible
- ✅ Rotate API keys and OAuth tokens periodically
- ✅ Keep your n8n instance behind authentication and a firewall

---

## 📝 Full Environment Variables Reference

| Placeholder | Description |
|---|---|
| `YOUR_OUTLOOK_CREDENTIAL_ID` | n8n Microsoft Outlook OAuth2 credential ID |
| `YOUR_OPENAI_CREDENTIAL_ID` | n8n OpenAI credential ID |
| `YOUR_GSHEETS_CREDENTIAL_ID` | n8n Google Sheets OAuth2 credential ID |
| `YOUR_HUBSPOT_CREDENTIAL_ID` | n8n HubSpot App Token credential ID |
| `YOUR_GOOGLE_SHEET_ID` | Google Sheet document ID |
| `YOUR_SALES_SHEET_GID` | Google Sheet tab GID for Sales tab |
| `YOUR_FOLLOWUP_SHEET_GID` | Google Sheet tab GID for follow_up tab |
| `YOUR_HUBSPOT_OWNER_ID` | HubSpot numeric deal owner ID |
| `YOUR_DOMAIN.com` | Your company email domain |
| `YOUR_COMPANY_NAME` | Your company name for AI signatures |
| `YOUR_OUTLOOK_INBOX_FOLDER_ID` | Outlook inbox folder ID (Graph API) |
| `YOUR_OUTLOOK_FOLDER_ID_ADMIN` | Admin department folder ID |
| `YOUR_OUTLOOK_FOLDER_ID_ACCOUNTING` | Accounting department folder ID |
| `YOUR_OUTLOOK_FOLDER_ID_COO` | COO department folder ID |
| `YOUR_OUTLOOK_FOLDER_ID_CEO` | CEO department folder ID |
| `YOUR_OUTLOOK_FOLDER_ID_CUSTOMER_CARE` | Customer Care folder ID |
| `YOUR_OUTLOOK_FOLDER_ID_SALES` | Sales department folder ID |

---

## 📄 License

This project is proprietary and intended for internal deployment. Not for public redistribution.

---

*Built with ❤️ using n8n, OpenAI GPT-4o, Microsoft 365 & HubSpot — automating intelligent client communication at scale.*
