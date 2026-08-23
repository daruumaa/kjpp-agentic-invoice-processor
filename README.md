# Intelligent Financial Document Processing Workflow (n8n + Gemini AI)

![n8n Workflow Screenshot](screenshot/image.png)

An advanced, automated n8n workflow designed to fetch, classify, and extract structured data from various financial documents using Google Gemini's multimodal Vision AI.

This workflow acts as an end-to-end automated accounting assistant. It monitors incoming files, intelligently identifies the document type (Invoices, Purchase Orders, Remittance documents, etc.), extracts the relevant data using specialized AI agents, and logs the structured outputs directly into specific Google Sheets.

---

## Key Features

- **Automated Ingestion** — Triggers via Webhook or batch-processes files directly from Google Drive.
- **Smart PDF Handling** — Automatically downloads and splits multi-page PDFs for accurate page-by-page processing.
- **AI-Powered Classification** — Uses a primary Gemini AI Classifier Agent to determine the exact nature of the document.
- **Specialized Vision Agents** — Routes documents to dedicated AI extraction agents based on their classification:
  - Invoices
  - Purchase Orders (PO)
  - Remittance Advice
  - Remittance Instructions
  - Bukti Transfer (Transfer Receipts)
- **Structured Output & Chart of Accounts** — Ensures AI hallucinations are minimized by forcing outputs into strict JSON schemas mapped to a Chart of Accounts.
- **Automated Database Logging** — Updates dedicated Google Sheets for each document type, culminating in a centralized "Master Log" update.

---

## Workflow Architecture: How It Works

### 1. Ingestion & Pre-processing
The workflow begins by searching for specific files in Google Drive and cross-referencing a Google Sheet. It loops through each item, downloads the file, and — if it's a PDF — splits it into digestible pages. Initial metadata is extracted and appended to a tracking sheet.

### 2. AI Classification (The "Brain")
The pre-processed document is fed into the **Classifier Agent** (powered by Google Gemini). This agent acts as a router, analyzing the document's content and tagging it with a specific financial category.

### 3. Conditional Routing (The "Switch")
Based on the Classifier's output, a `Switch` node directs the document down one of several specialized paths. Unrecognized documents are routed to a manual review queue.

### 4. Specialized AI Extraction
Each distinct document type hits a specialized Agent (Vision). These multimodal agents read the visual and textual data of the document to extract critical fields (e.g., vendor names, totals, line items, dates). They utilize a `Structured Output Parser` to ensure the data is perfectly formatted.

### 5. Data Synchronization
Once the data is extracted, it is routed to the corresponding Google Sheets node:

- `Update Booking List`
- `Update Purchase Order`
- `Update Remittance Instruction`
- `Update Remittance Advice`
- `Update Bukti Transfer`

### 6. Master Logging
All branches merge back together at the end of the pipeline to update a unified **Master Log** sheet, ensuring a complete audit trail of every processed document.

---

## Prerequisites & Credentials

To run this workflow in your own n8n instance, you will need the following credentials configured:

- **Google Drive API** — For searching and downloading the source documents.
- **Google Sheets API** — For reading tracking sheets and writing the final extracted data.
- **Google Gemini API** — An active API key for the Gemini Chat/Vision models to power the Classifier and Extraction Agents.

---

## Setup Instructions

1. Clone this repository or download the `workflow.json` file.
2. Open your n8n instance.
3. Click **Add Workflow** in the top right corner.
4. Select **Import from File** and upload the `workflow.json` file.
5. Open the workflow and double-click the nodes with credential warnings to select your configured Google and Gemini credentials.
6. Update the Google Drive Folder IDs and Google Sheet IDs within the nodes to match your personal Workspace environment.
7. Click **Save** and toggle the workflow to **Active**.

---

Built with [n8n](https://n8n.io) and [Google Gemini](https://ai.google.dev).
