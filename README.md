🚀 Signal-to-Action: AI-Powered Lead Triage & Hyper-Personalization Engine
A resilient GTM (Go-To-Market) automation middleware built with n8n, Gemini 1.5, and Airtable. This engine automates the entire lead lifecycle: from raw form capture and surgical data normalization to AI-driven tiering and contextual outreach hooks.

🛠️ The Challenge
Inbound lead data is notoriously "dirty" (e.g., "ZoomInfo LLC", "Apple Inc."). Standard automations often fail due to:

Data Noise: Legal suffixes (LLC, Pvt Ltd) ruining personalization.

LLM Inconsistency: AI models returning Markdown-wrapped JSON that breaks downstream parsers.

Prioritization Gaps: Sales teams wasting time on SMB leads while Enterprise leads sit in a queue.

🏗️ Technical Architecture
This workflow implements a multi-path asynchronous architecture:

Ingestion & Staging: Captures raw data via Webhooks/Tally.so and creates an initial record in Airtable to generate a unique UID.

Surgical Normalization (JS): A custom JavaScript node uses regex-based boundary awareness to strip corporate suffixes (Pvt, Ltd, LLC, Pte) without mangling internal strings (e.g., "Apteau").

AI Orchestration (Gemini): Processes normalized data to perform:

Tiering: Assigning Lead Tiers (A/B/C) based on firmographic logic.

Personalization: Generating a 2-sentence "Business Hook" tailored for Wati’s WhatsApp API context.

Resilient Sanitization (JS): A dedicated "Sanitizer" node extracts raw JSON from LLM responses using regex, ensuring the workflow is immune to Markdown formatting errors.

Data Synchronization: Uses a 3-way Merge logic to sync all enriched attributes back to the primary CRM record.

Action Layer (PingHook): Triggers real-time alerts via PingHook (Webhook-to-Telegram) for instant sales visibility.

🧰 Tech Stack
Automation: n8n (Self-hosted)

Intelligence: Google Gemini (via LangChain)

Database: Airtable (Acting as a Headless CRM)

Scripting: Node.js / JavaScript (Regex-heavy for data integrity)

Alerting: PingHook API

📈 GTM Impact
Reduces Sales Research Time: Automated "First Lines" mean reps spend 0 minutes researching "hooks."

Data Integrity: Ensures CRM records are clean for automated WhatsApp/Email templates.

Lead Velocity: High-tier leads are flagged and tiered in < 5 seconds from form submission.

🔧 Installation & Setup
Import the workflow.json into your n8n instance.

Configure your Airtable Base and Gemini API credentials.

Set your PingHook endpoint in the final HTTP Request node.
