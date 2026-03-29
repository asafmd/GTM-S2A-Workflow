# 🚀 Signal-to-Action: AI-Powered Lead Triage & Hyper-Personalization Engine

A resilient GTM (Go-To-Market) automation middleware built with **n8n**, **Gemini 1.5**, and **Airtable**. This engine automates the entire lead lifecycle: from raw form capture and surgical data normalization to AI-driven tiering and contextual outreach hooks.

## 🛠️ The Challenge

Inbound lead data is notoriously "dirty" (e.g., "ZoomInfo LLC", "Apple Inc."). Standard automations often fail due to:
1. **Data Noise:** Legal suffixes (LLC, Pvt Ltd) ruining personalization.
2. **LLM Inconsistency:** AI models returning Markdown-wrapped JSON that breaks downstream parsers.
3. **Prioritization Gaps:** Sales teams wasting time on SMB leads while Enterprise leads sit in a queue.

## 🏗️ Technical Architecture

This workflow implements a **multi-path asynchronous architecture**:

1. **Ingestion & Staging:** Captures raw data via Webhooks/Tally.so and creates an initial record in Airtable to generate a unique UID.
2. **Surgical Normalization (JS):** A custom JavaScript node uses regex-based boundary awareness to strip corporate suffixes (Pvt, Ltd, LLC, Pte) without mangling internal strings (e.g., "Apteau").
3. **AI Orchestration (Gemini):** Processes normalized data to perform:
    - **Tiering:** Assigning Lead Tiers (A/B/C) based on firmographic logic.
    - **Personalization:** Generating a 2-sentence "Business Hook" tailored for **Wati’s WhatsApp API** context.
4. **Resilient Sanitization (JS):** A dedicated "Sanitizer" node extracts raw JSON from LLM responses using regex, ensuring the workflow is immune to Markdown formatting errors.
5. **Data Synchronization:** Uses a 3-way **Merge** logic to sync all enriched attributes back to the primary CRM record.
6. **Action Layer (PingHook):** Triggers real-time alerts via **PingHook** (Webhook-to-Telegram) for instant sales visibility.

## 🧰 Tech Stack

* **Automation:** n8n (Self-hosted)
* **Intelligence:** Google Gemini (via LangChain)
* **Database:** Airtable (Acting as a Headless CRM)
* **Scripting:** Node.js / JavaScript (Regex-heavy for data integrity)
* **Alerting:** PingHook API (Webhook-to-Telegram)

## 📈 GTM Impact

* **Reduces Sales Research Time:** Automated "First Lines" mean reps spend 0 minutes researching "hooks."
* **Data Integrity:** Ensures CRM records are clean for automated WhatsApp/Email templates.
* **Lead Velocity:** High-tier leads are flagged and tiered in < 5 seconds from form submission.

---

## 🔧 Installation & Setup

1. **Import:** Download the `workflow.json` from this repo and import it into your n8n instance.
2. **Credentials:** - Set up an **Airtable** Personal Access Token.
    - Set up a **Google Gemini** API key.
3. **PingHook:** Replace the PingHook URL with your specific endpoint for Telegram notifications.
4. **Execute:** Trigger the workflow via the Tally.so webhook or the built-in n8n form.

---

## 💡 Lessons Learned & Engineering Notes

* **Regex vs. Simple Replace:** Using `\b` word boundaries and end-of-line anchors `$` was critical to avoid "Apteau" becoming "Aau" when stripping "Pte".
* **State Management:** By creating the Airtable record *first*, we ensure we have an immutable ID to anchor all subsequent AI and enrichment data to, preventing race conditions.
* **LLM Resilience:** Implementing a `try/catch` block within the JSON sanitizer ensures that even if the AI hallucinates or shifts format, the system flags the error instead of crashing the pipeline.

---
*Developed by Asaf Mohammed for AlphaNova AI & GTM Engineering Portfolio.*
