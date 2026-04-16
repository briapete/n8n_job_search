## 🧩 Airtable "Command Center" Logic

This document outlines the backend configuration of the **Job Search Tracker** base, which serves as the relational database and user interface for the n8n automation pipeline.

### 🔌 Data Integration from n8n
* [cite_start]**Ingestion:** Data is sent to Airtable using the `Airtable: Create Row` node in n8n[cite: 17].
* [cite_start]**Field Mapping:** The workflow populates key metrics including **Match Score**, **Salary Range**, and **Job Description** directly into the "Search Results" table[cite: 16].
* **Relational Schema:** The base tracks critical job metadata including timestamps, company names, locations, and direct application links.

![Database Schema](https://github.com/briapete/n8n_job_search/blob/feature_airtable_doc/docs/assets/airtable_table.png?raw=true)

---

### 🤖 Automation: Inbound Lead Processing
I developed a webhook-driven notification system to alert me as soon as a batch of jobs has been processed and scored by the AI.

* **Trigger:** `When Webhook Received` — This endpoint listens for a final POST request from the n8n Gemini Dispatcher signaling that the evaluation batch is complete.
* **Action:** `Gmail: Send email` — Automatically dispatches a digest to my inbox.
* **Integrated UX:** The email body is dynamically generated to include a direct link to the **Job Evaluation Interface**, enabling an immediate transition from notification to vetting.

![Webhook Automation Flow](https://github.com/briapete/n8n_job_search/blob/feature_airtable_doc/docs/assets/Send%20Email%20Automation.png?raw=true)

---

### 📊 Custom Interface Design
To solve the problem of volume, I built a high-density interface that prioritizes my "Tier A" opportunities.

* **Key Metrics Dashboard:** Provides an "at-a-glance" view of the total jobs found, the count of **Tier A Opportunities**, and the **Average Match Score** across the current batch.
* **Tiered Segmentation:** The interface automatically filters roles into Tier A, B, and C. In the current iteration, the system has identified **44 Tier A Opportunities** with an average match score of **90**.
* **Candidate Detail View:** Each record expands into a rich detail page showing the **AI-generated Notes**, providing a qualitative fit analysis based on my 22+ years of experience.

| Interface View | Description |
| :--- | :--- |
| **Search Results Statistics** | High-level metrics for batch health monitoring. |
| **Tier A Opportunities** | A prioritized list of roles with a Match Score > 90. |
| **Individual Job Detail** | Deep-dive view including Salary, Match Score, and AI reasoning. |

![Interface Dashboard](https://github.com/briapete/n8n_job_search/blob/feature_airtable_doc/docs/assets/Job%20Tracker%20Interface.png?raw=true)

---

### 🔢 Schema Highlights
* [cite_start]**Automated Match Scoring:** Utilizes weighted formulas to compare job requirements against my verified resume data, including contract values and timeline lengths[cite: 11, 15].
* **Data Normalization:** Salary data is processed to handle inconsistent formatting (e.g., converting "80K–100K a year" into numeric ranges).
* **Dynamic Notes:** Captures the "AI's reasoning" for each score to provide immediate context during the vetting process.