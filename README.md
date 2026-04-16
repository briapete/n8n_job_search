# AI Career Orchestrator: Automated Job Discovery and Evaluation
> [!NOTE]  
> ⚡ **Airtable Integration:** This workflow now supports **Airtable** alongside Google Sheets for real-time job tracking.
This repository contains a suite of n8n workflows designed to automate the end-to-end process of job searching. The system uses a Dispatcher-Worker architecture to scale job searches, evaluate fit using LLMs, and maintain a live lead tracker in Google Sheets.

This repository contains a suite of n8n workflows designed to automate the end-to-end process of job searching. The system uses a Dispatcher-Worker architecture to scale job searches, evaluate fit using LLMs, and maintain a live lead tracker in Google Sheets.

## System Architecture

The project is split into three core functional layers:

1. Orchestrator (Main Agent): Interprets user chat requests and delegates tasks to specialized tools.
2. Search Workers: Specialized sub-workflows for Adzuna and Google Jobs that handle API pagination and data normalization.
3. Evaluation Engine: A high-precision LLM chain that scores job descriptions against a user resume and assigns a priority Tier (A, B, or C).

## Workflow Components

### 1. Main Agent (Job Search with Vertex.json)
* Role: The central controller for the automation.
* Key Feature: Uses Gemini 2.0 (Vertex AI) to manage the conversation and trigger tools.
* Stability Logic: Configured with specific system prompts to prevent infinite reasoning loops by delegating bulk processing to the worker workflows.

### 2. Adzuna Worker (Job Search Tool Aadzuna.json)
* Input: job_title, location, and results_per_page.
* Processing: Fetches raw data from the Adzuna API, flattens the results, and loops through each job for evaluation.
* The Handshake: Includes an Edit Fields node that returns a structured message and success flag back to the AI Agent. This ensures the Parent Agent maintains synchronization during long-running batches.

### 3. Google Worker (Job Search Tool Google.json)
* Input: job_title, location, and page.
* Processing: Utilizes SerpAPI to extract Google Jobs results, normalizing the data for the evaluation engine.

### 4. Evaluation Worker (Process Job Descriptions.json)
* Logic: Compares a provided resume against a job description.
* Output: Generates a structured JSON object containing a tier, reason, and match_score.

## Setup Instructions

1. Google Sheets: Create a sheet named "Job Tracker Search" with headers for Link, Company, Tier, Match Score, and Notes.
2. n8n Credentials: Set up credentials for:
    * Google Vertex AI (for the Agent and Evaluation).
    * Adzuna API (for job data).
    * SerpAPI (for Google Jobs search).
    * Google Sheets (for data persistence).
3. Import: Import all .json files into your n8n instance and update the Execute Workflow nodes to point to the correct internal Workflow IDs.

## Future Roadmap (TODO)
* Automated Outreach: Add a sub-workflow to draft personalized LinkedIn messages or cover letters based on the LLM "Match Score" notes.
* Automated Versioning: Integration with n8n's Git-sync for automated production deployments.
* Additional Job Search Engines: Incorporate additional repositories for diverse job opportunities
* Job Description Analysis: Include advanced logic for job evaluation against personal preferences.
