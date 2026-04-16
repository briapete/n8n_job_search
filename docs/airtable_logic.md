## Airtable "Command Center" Logic

This document outlines the backend configuration of the **Job Search Tracker** base, acting as the relational database and UI for the n8n automation pipeline.

### Data Integration from n8n
* [cite_start]**Ingestion:** Data is sent to Airtable using the `Airtable: Create Row` node in n8n[cite: 4].
* [cite_start]**Field Mapping:** The workflow populates key metrics including **Match Score**, **Salary Range**, and **Job Description** directly into the Search Results table.

### Automation: Inbound Lead Processing
When the n8n evaluation is complete, a secondary internal Airtable automation handles the user notification:
* **Trigger:** `When Webhook Received` — This endpoint listens for a final HTTP request from n8n signaling the batch is complete.
* **Action:** `Send Email` — Automatically sends a digest notification with a link to the **Airtable Interface**, allowing for immediate triage of new postings.

### Schema Highlights
* [cite_start]**Automated Match Scoring:** Utilizes weighted formulas to compare job requirements against my background in platform integrations and AI workflows[cite: 4].
* [cite_start]**Data Normalization:** Salary data is normalized via regex-based formulas to handle inconsistent formatting (e.g., converting "80K–100K" into a numeric range)[cite: 3].
