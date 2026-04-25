
# 📬 AI-Powered Email Automation & Lead Intelligence System

> Built for **Germany-based consultancy company** — specializing in operations and hospitality consulting.

![n8n](https://img.shields.io/badge/n8n-Workflow_Automation-orange?style=for-the-badge&logo=n8n)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o-412991?style=for-the-badge&logo=openai)
![Microsoft Outlook](https://img.shields.io/badge/Microsoft-Outlook-0078D4?style=for-the-badge&logo=microsoft-outlook)
![HubSpot](https://img.shields.io/badge/HubSpot-CRM-FF7A59?style=for-the-badge&logo=hubspot)
![Google Sheets](https://img.shields.io/badge/Google-Sheets-34A853?style=for-the-badge&logo=google-sheets)

---


<img width="1873" height="707" alt="image" src="https://github.com/user-attachments/assets/dacb7eb2-75c8-4038-bf4f-1b6bfecdb768" />


---
## 🚨 Problem Statement

The consultancy was receiving **dozens of inbound emails daily** across multiple departments — Sales, Admin, Accounting, COO, CEO, and Customer Care. The existing process was entirely manual and unsustainable:

| Pain Point | Impact |
|---|---|
| ❌ 100% manual classification | Staff hours wasted on triage |
| ❌ No lead scoring or prioritization | Hot leads buried under noise |
| ❌ Zero CRM sync | Contact data scattered across inboxes |
| ❌ No audit trail | Zero visibility on inbound volume or trends |
| ❌ Inconsistent replies | Brand perception degraded per-sender |
| ❌ No follow-up scheduling | Missed touchpoints and lost revenue |

**The result:** delayed response times, lost business opportunities, and an inconsistent client experience across every department.

---

## ✅ Solution Overview

An end-to-end **AI-native n8n automation** that intercepts every inbound email, understands its intent using GPT-4o, routes it to the correct department, drafts and sends a professional reply, scores the lead, syncs to HubSpot CRM, and logs everything to Google Sheets — **autonomously, 24/7, with zero human bottleneck**.

**What changed after deployment:**

- ✅ Response time dropped from hours → under 2 minutes
- ✅ Zero missed hot leads — immediate sales team alerts
- ✅ 100% CRM coverage — every contact auto-captured
- ✅ Full audit log — timestamped, searchable, traceable
- ✅ Consistent professional tone across all departments

---


## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        MICROSOFT OUTLOOK TRIGGER                        │
│                    (Polls every 60 seconds for new mail)                │
└─────────────────────────┬───────────────────────────────────────────────┘
                          │
                          ▼
              ┌─────────────────────┐
              │  BATCH SPLIT + DATE │  ← Processes 1 email at a time
              │       FILTER        │    to prevent data contamination
              └──────────┬──────────┘
                         │
               ┌─────────┼─────────┐
               ▼         ▼         ▼
         Self-Email   Booking   Proceed
           Filter     Filter       │
           (STOP)     (STOP)       │
                                   ▼
                    ┌──────────────────────────┐
                    │   REPLY vs NEW DETECTOR  │
                    └──────────┬───────────────┘
                               │
                    ┌──────────┴───────────┐
                    ▼                      ▼
               Reply Thread           New Email
                    └──────────┬───────────┘
                               ▼
                    ┌──────────────────────┐
                    │  HUMAN REQUEST CHECK │  → Human? Alert staff immediately
                    └──────────┬───────────┘
                               │ (AI handles it)
                               ▼
                    ┌──────────────────────┐
                    │  FIELD NORMALIZE +   │
                    │    EDIT FIELDS       │
                    └──────────┬───────────┘
                               ▼
                    ┌──────────────────────┐
                    │  GPT-4o SUMMARIZER   │  ← 1-2 sentence digest
                    └──────────┬───────────┘
                               ▼
                    ┌──────────────────────┐
                    │  GPT-4o DEPARTMENT   │  ← Routes to correct team
                    │      ROUTER          │
                    └──────────┬───────────┘
                               │
          ┌────────────────────┼─────────────────────┐
          ▼        ▼           ▼         ▼            ▼          ▼
       ADMIN   ACCOUNTING    COO        CEO         SALES    CUSTOMER
          │        │           │         │            │         CARE
          │        │           │         │            │            │
          └────────┴───────────┴─────────┘            │            │
                        │                             │            │
               ┌────────┴────────┐                   ▼            │
               │ AI Reply Draft  │           ┌──────────────┐     │
               │ Staff Alert     │           │ LEAD SCORING │     │
               │ Folder Move     │           │   (0-100)    │     │
               │ Mark Read       │           └──────┬───────┘     │
               └─────────────────┘                  │             │
                                            ┌───────┴────────┐    │
                                            │  Hot >= 70?    │    │
                                            │  Warm >= 40?   │    │
                                            │  Cold  < 40?   │    │
                                            └───────┬────────┘    │
                                                    │             │
                                          ┌─────────┴──────────┐  │
                                          │   HUBSPOT CRM SYNC │  │
                                          │  Contact + Deal    │  │
                                          │  Pipeline Stage    │  │
                                          └─────────┬──────────┘  │
                                                    │             │
                                          ┌─────────┴──────────┐  │
                                          │  CLIENT EMAIL SENT │◄─┘
                                          └─────────┬──────────┘
                                                    │
                                          ┌─────────▼──────────┐
                                          │  GOOGLE SHEETS LOG │
                                          │  (3 tabs tracked)  │
                                          └────────────────────┘
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

## ✨ Core Features

### 📥 Smart Email Ingestion

The pipeline starts with a hardened ingestion layer that filters noise before AI processing:

- Polls the monitored Outlook inbox **every 60 seconds**
- Splits batch results into individual items to **prevent data contamination** between concurrent emails
- Applies a **date filter** to avoid reprocessing historical mail
- Silently drops **self-sent emails** (no processing, no reply)
- Silently drops **calendar invites and booking notifications**
- Detects whether the email is a **new conversation or a reply thread** (different processing paths)

---

### 🧠 AI-Powered Understanding (GPT-4o)

Every email passing the ingestion layer is analyzed by GPT-4o in two sequential steps:

**Step 1 — Summarization**
Generates a 1–2 sentence plain-language summary of the email's intent, regardless of language or formatting.

**Step 2 — Department Routing**
Reads the summary and full email context, then outputs a structured routing decision. Fallback for ambiguous emails always defaults to **Sales** to maximize lead capture.

> GPT-4o handles multilingual emails natively — relevant for a Germany-based client receiving both English and German correspondence.

---

### 🏢 Department Routing Engine

Six fully independent department branches, each with its own AI agent:

| Department | Routing Trigger Context | Branch Behavior |
|---|---|---|
| **Sales** *(default)* | Pricing, demos, interest, general inquiries | Lead scored, HubSpot synced, reply sent |
| **Admin** | HR, internal office requests, access | Staff notified, folder moved |
| **Accounting** | Invoices, payments, billing, refunds | Staff notified, folder moved |
| **COO** | Operations, process improvements, logistics | Staff notified, folder moved |
| **CEO** | Strategy, investment, board-level matters | Staff notified, folder moved |
| **Customer Care** | Existing customer issues, complaints | Personalized reply, folder moved |

**Every branch executes the same 4 post-routing actions:**

1. ✅ AI-generated professional reply (department-specific tone and signature)
2. ✅ Internal staff notification email dispatched
3. ✅ Email moved to the correct Outlook department folder
4. ✅ Message flagged as read in the inbox

---

### 🔥 Lead Scoring Engine (Sales Branch)

A deterministic, keyword-based scoring model that assigns every inbound sales email a score from **0 to 100** and classifies it into one of three buckets:

**Scoring Matrix:**

| Intent Signal | Trigger Keywords | Score Modifier |
|---|---|---|
| 🔥 Purchase Intent | buy, pricing, cost, get started, subscribe | **+50** |
| 🎯 Demo / Trial Interest | demo, trial, interested, walkthrough | **+40** |
| ⚡ Information Seeking | details, explain, how, what | **+15** |
| 🏢 Service Inquiry | service, solution, consultation | **+20** |
| ❄️ Low-Value Contact | just checking, hi, hello | **−5** |

**Lead Classification Thresholds:**

```
Score >= 70  →  🔥 HOT LEAD    — Immediate alert to sales team
Score >= 40  →  🌡️ WARM LEAD   — Standard pipeline entry
Score <  40  →  ❄️ COLD LEAD   — Logged, no priority alert
```

> **Hot Lead Alert** triggers an immediate email notification to the designated sales owner — ensuring response within minutes, not hours.

---

### 🤝 HubSpot CRM Integration

Every qualified Sales lead is fully synced to HubSpot in a 4-step sequence:

1. **Contact Search** — checks if contact already exists by email
2. **Contact Create / Update** — name, email, phone, lead score, and AI summary written to the contact record
3. **Deal Search** — checks for an existing open deal linked to the contact
4. **Deal Create / Update** — pipeline stage set automatically based on lead score:
   - Hot Lead → `qualifiedtobuy`
   - Warm Lead → `appointmentscheduled`

All deals are assigned to the configured deal owner ID. No manual CRM entry ever required.

---

### 📊 Google Sheets Audit & Logging

Three structured tabs maintained automatically in real time:

**Tab 1 — `Sheet1` (General Log)**

| Column | Description |
|---|---|
| Message ID | Unique Outlook message identifier |
| Sent TO | Recipient department or address |
| Status | Processing outcome |
| Time | Timestamp of processing |
| User_Message | Original message preview |

**Tab 2 — `Sales`**

| Column | Description |
|---|---|
| Name | Sender full name |
| Email | Sender email address |
| Lead Score | Computed 0–100 score |
| M_Id | Linked message ID |
| Created_At | Timestamp |

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

