# 📧 AI-Powered Email Automation Agent

This repository contains an AI-driven email automation agent built using **n8n** workflows. This agent is designed to significantly reduce manual email triage, speed up internal response times, and ensure important messages are routed efficiently to the correct teams or systems.

It leverages the **Gemini API** for sophisticated Natural Language Processing (NLP) and context-aware response drafting, orchestrated seamlessly via **n8n**.

## 🚀 Project Overview

The core function of this agent is to intelligently process incoming emails from a designated Gmail account. The process involves:

1.  **Ingestion & Classification:** Reading new Gmail messages and classifying the sender category (e.g., Client, Vendor, Internal, Spam).
2.  **NLP & Summarization:** Utilizing the Gemini API to summarize long or unstructured emails, extract intent, and identify key action items.
3.  **Actioning & Response:** Based on classification and content analysis, the system drafts responses, labels/moves the email, or triggers downstream systems.
4.  **Human-in-the-Loop:** Implementing approval workflows for sensitive or high-impact replies.

### 💡 Key Technologies

| Technology | Role |
| :--- | :--- |
| **n8n** | **Orchestration Layer:** Triggers, data transformation, API calls (Gemini, Gmail), database writes, workflow management. |
| **Gemini API** | **Intelligence Layer:** Provides summarization, intent classification, and context-aware response drafting. |
| **Gmail API** | **Integration:** New email triggers, actions (send, label, move, reply). |
| **Node.js (n8n)** | The runtime environment for the n8n automation server. |
| **SQL** | **Data Persistence:** Stores processed email metadata, summaries, response decisions, and conversation logs. |

## ✨ Features

* **Intelligent Gmail Integration:** Read, label, move, reply, and modify incoming email messages.
* **Sender-Based Routing & Rules:** Custom rules for classifying and prioritizing messages from Clients, Vendors, Internal teams, or other categories.
* **Advanced NLP:**
    * **Summarization:** Condense lengthy emails into digestible summaries.
    * **Intent Extraction:** Automatically determine the purpose of the email (e.g., Support Request, Invoice Inquiry, Feature Request).
    * **Action Items:** Identify explicit or implied next steps.
* **Context-Aware Auto-Drafting:** Generate draft responses using the Gemini API based on configurable templates and extracted email intent.
* **Human-in-the-Loop (HITL) Approval:** Essential approval flows for sensitive or critical replies, preventing automated errors.
* **Data Persistence:** Persisted metadata, summaries, and conversation logs in a SQL database for auditing and analysis.
* **Webhooks for Downstream Systems:** Seamless integration with ticketing systems, Slack notifications, CRMs, etc.

## 🏗️ Architecture (High-Level)

The agent operates across four key components coordinated by n8n. 

| Component | Function |
| :--- | :--- |
| **n8n (Orchestrator)** | Manages the entire workflow: triggers on new emails, calls Gemini, transforms data, executes conditional logic, and writes logs to the Database. |
| **Gmail API** | Acts as the **Trigger** (new email/watch) and the **Actioner** (sending replies, labeling, filing). |
| **Gemini AI** | The **Intelligence** source: takes email content as input and outputs classification, summary, or a draft response. |
| **Database (SQL)** | The **Persistence** layer: stores all processed data, logs, and decision history for auditing and analysis. |

## ⚙️ Setup and Installation

### Prerequisites

* A running **n8n instance** (self-hosted or cloud).
* A **Gmail account** and **Google Cloud Project** for the Gmail API credentials.
* An **API Key** for the **Gemini API**.
* A **SQL Database** instance (PostgreSQL, MySQL, etc.) for logging.

### 1. Configure Credentials

Add the following credentials to your n8n instance:

* **Gmail Account:** OAuth2 credentials for accessing the Gmail API (scope: `https://www.googleapis.com/auth/gmail.modify` or similar).
* **Gemini API Key:** A credential using your Gemini API Key.
* **Database:** A credential for your chosen SQL database (e.g., PostgreSQL, MySQL).

### 2. Import the n8n Workflow

1.  Navigate to the n8n Editor.
2.  Click **"New"** then **"Import from JSON"**.
3.  Import the workflow JSON file (you will need to create this from your n8n design).

### 3. Customize the Workflow

The imported workflow will contain nodes that need configuration:

* **Gmail Trigger:** Ensure it is watching the correct mailbox or label.
* **Gemini Nodes (Classification/Summarization):** Update the prompts to match your specific classification categories (e.g., `Client`, `Vendor`, `Internal`).
* **Database Node:** Verify the connection details and the structure of the tables used for logging.
* **Response Drafting:** Customize the prompt templates used by Gemini to draft replies to ensure the tone and style align with your organization.

## 💻 Usage

Once the workflow is active:

1.  A new email arrives in the monitored Gmail account.
2.  The **Gmail Trigger** fires the n8n workflow.
3.  The workflow first sends the email content to Gemini for **Classification**.
4.  Based on the classification (e.g., `Client`), a specific path is taken:
    * **Path: Client Email** -> Send to Gemini for **Summarization** & **Drafting**.
    * **Path: Vendor Email** -> Log to DB and send a Slack alert via a webhook.
    * **Path: Spam** -> Label and archive the email.
5.  If a draft is created, it enters the **Human-in-the-Loop** flow (e.g., sends a request to a manager via a webhook with the draft text).
6.  Upon approval, the n8n workflow uses the **Gmail Node** to send the final reply.
7.  All steps and decisions are logged in the **SQL Database**.

## 🤝 Contributing

Contributions are welcome! Please open an issue first to discuss the change you would like to make.

1.  Fork the repository.
2.  Create your feature branch (`git checkout -b feature/AmazingFeature`).
3.  Commit your changes (`git commit -m 'Add some AmazingFeature'`).
4.  Push to the branch (`git push origin feature/AmazingFeature`).
5.  Open a Pull Request.

<img width="800" height="507" alt="image" src="https://github.com/user-attachments/assets/8be67361-d686-4187-b20e-20781696abd4" />
