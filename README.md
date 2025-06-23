# Project: The Autonomous Sales & Marketing Assistant

A resilient, AI-powered n8n workflow designed to automate the most time-consuming part of sales: lead research and enrichment. This agent autonomously scrapes company websites, analyzes the content with AI, and delivers concise, actionable intelligence.

This project was born out of a real-world challenge: simple web scrapers are no longer enough. This workflow is engineered to overcome modern web-scraping obstacles and turn raw data into valuable insight, completely hands-free.

---

## Core Features

-   **Intelligent Dual-Path Scraping:** The workflow first attempts a free, direct scrape. If it detects a block page or failure, it automatically triggers a professional-grade fallback scraper (ScraperAPI) to ensure data is always retrieved.
-   **AI-Powered Summarization:** Raw HTML is piped directly to Google's Gemini Pro model, which acts as a business analyst. It extracts the company's headline, tagline, and a one-sentence summary, transforming thousands of characters of code into actionable intelligence.
-   **Autonomous Triggering:** The entire process is initiated simply by adding a new company name and website to a Google Sheet, making it effortlessly scalable.
-   **Resilient Error Handling:** The workflow is designed to correctly identify failures at multiple stages, from the initial scrape to the AI analysis, and log the status accordingly.
-   **Clean Data Logging:** All results are neatly logged in a separate "Results" sheet, creating a clean, organized database of enriched leads without altering the original input data.

---

## Our Workflow Architecture

The power of this agent lies in its intelligent, branching structure:

1.  **Trigger (`Google Sheets Trigger`):** Activates when a new row is added to the input sheet.
2.  **Attempt #1 (`HTTP Request`):** Performs a direct, standard web scrape.
3.  **The Brain (`If` Node):** This is the crucial step. It checks if the first attempt produced an error.
    -   **If `error` exists (TRUE Path):** The workflow proceeds to the professional scraper.
    -   **If no `error` exists (FALSE Path):** The workflow proceeds with the successful direct scrape.
4.  **Attempt #2 (`ScraperAPI Request`):** The fallback path. This node uses the ScraperAPI service to handle JavaScript-heavy sites and bypass anti-bot measures.
5.  **The Analyst (`Google Gemini`):** The successful HTML from either path is fed to the Gemini API with a specific prompt to analyze the content.
6.  **Data Assembly (`Set` Node):** Gathers the original company info from the trigger and the new AI-generated summary into a single, clean data object.
7.  **Logging (`Google Sheets`):** Appends the final, clean data object as a new row in the "Results" sheet.

---

## Technology Stack

-   **Automation Platform:** [n8n.io](https://n8n.io/) (Self-Hosted)
-   **AI Model:** Google Gemini Pro
-   **Primary Data Store:** Google Sheets
-   **Fallback Scraping Service:** [ScraperAPI](https://scraperapi.com/)
-   **Core Logic:** Custom n8n expressions and workflow branching.

---

## Setup & Configuration

To replicate this workflow:
1.  Set up the `Sheet1` (input) and `Results` (output) tabs in a Google Sheet.
2.  Import the workflow JSON file into your n8n instance.
3.  Create credentials in n8n for:
    -   Google Sheets (using OAuth)
    -   Google Gemini
    -   A `Header Auth` credential for ScraperAPI, using your API key.
4.  Connect the credentials to the appropriate nodes.
5.  Activate the workflow.
