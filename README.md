# 🎯 Influencer Marketing Lead Qualification Engine

An automated workflow that identifies brands actively investing in influencer marketing and classifies them as **Sales Qualified Leads (SQLs)** — without any manual Instagram review.

---

## 📌 Overview

Manually reviewing hundreds of brand Instagram profiles to detect influencer collaboration activity was time-intensive and inconsistent. This system automates the entire process end-to-end: from reading a list of brand profiles in Google Sheets, scraping their Instagram presence, analyzing recent content for collaboration signals, and writing a qualified lead status back to the sheet — all without human intervention.

---

## 🏗️ Architecture

```
Google Sheets → n8n Workflow → Apify Instagram Scraper → JS Qualification Engine → Google Sheets Update
```

**Pipeline at a glance:**

| Step | What Happens |
|------|-------------|
| 1 | Read brand Instagram URLs from Google Sheets |
| 2 | Parse and normalize Instagram handle from URL |
| 3 | Trigger Apify actor to scrape profile metrics |
| 4 | Poll for scrape completion, extract follower count |
| 5 | Filter out profiles below the follower threshold |
| 6 | Trigger second Apify actor to scrape latest 20 Reels |
| 7 | Analyze captions and metadata for collaboration signals |
| 8 | Count signals and apply qualification logic |
| 9 | Write SQL / Not SQL status back to Google Sheets |

---

## ⚙️ Qualification Logic

A lead is classified as **SQL** when both conditions are met:

- **Follower count ≥ 6,000**
- **3 or more collaboration signals** detected across the brand's latest 20 Reels

### Collaboration Signals Detected

**Caption keywords (paid partnership language):**
- `paid partnership`, `paid collaboration`, `gifted by`, `gifted from`
- `in collaboration with`, `in association with`, `sponsored by`, `partnered with`

**Sponsored hashtags:**
- `#ad`, `#paidpartnership`, `#paidcollaboration`, `#sponsored`
- `#gifted`, `#brandambassador`, `#collab`, `#promotion`

**Structural signals:**
- Instagram native Paid Partnership label (detected via post metadata)
- Collaboration posts featuring tagged influencer accounts

Leads that fail the follower threshold are marked **Not SQL** immediately with the skip reason logged, without consuming the Reels scrape quota.

---

## 🛠️ Tech Stack

| Tool | Role |
|------|------|
| **n8n** | Workflow orchestration and automation |
| **Apify** | Instagram profile + Reels scraping |
| **Google Sheets API** | Lead input source and output destination |
| **JavaScript** | Qualification logic, signal detection, state management |

---

## 📁 Repo Contents

```
├── SQL_Scraper_Final.json     # n8n workflow export (importable directly)
└── README.md
```

### Import the Workflow

1. Open your n8n instance
2. Go to **Workflows → Import from file**
3. Select `SQL_Scraper_Final.json`
4. Configure credentials:
   - Google Sheets OAuth2
   - Apify API Token (HTTP Request header)
5. Set your Google Sheet ID and tab name in the relevant nodes
6. Run

---

## 📊 Business Impact

- Eliminated manual Instagram review across hundreds of brand accounts
- Standardized SQL qualification criteria across the sales team
- Reduced per-lead research time significantly
- Enabled sales to focus pipeline effort on high-intent influencer marketing prospects

---

## 🔑 Key Skills Demonstrated

`Workflow Automation` · `Business Process Automation` · `JavaScript` · `API Integration` · `Lead Qualification Logic` · `Data Extraction` · `Sales Operations` · `Revenue Operations`

---

> Built and maintained as part of ongoing sales operations work. Actively used in production.
