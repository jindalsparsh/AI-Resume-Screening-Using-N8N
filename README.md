# AI-Resume-Screening-Using-N8N
An end-to-end AI-powered resume screening system built using n8n, OpenAI, and Google Workspace. This workflow automatically ingests resumes from email, normalizes multiple file formats, evaluates candidates against a job description using an AI recruiter agent, and logs structured hiring insights into Google Sheets.
✨ Key Capabilities

📥 Auto-ingest resumes from Gmail

📄 Supports PDF, DOCX, and TXT resumes

🔄 Normalizes all resumes into clean text

🧠 AI recruiter agent evaluates resume vs job description

📊 Structured scoring (skills, experience, fit, risk, reward)

🧾 Extracts candidate metadata (name, email)

📈 Appends results into Google Sheets

☁️ Stores resumes securely in Google Drive

🧱 High-Level Architecture
Gmail Trigger
   │
   ▼
Upload Resume → Detect File Type
   │
   ├── DOCX → Convert → PDF → Extract Text
   ├── PDF  → Extract Text
   └── TXT  → Extract Text
   │
   ▼
Standardized Resume Text
   │
   ▼
Fetch Job Description (Drive)
   │
   ▼
AI Recruiter Agent (LLM)
   │
   ├── Candidate Evaluation
   ├── Risk & Reward Analysis
   ├── Dimension-wise Scoring
   └── Overall Fit Rating
   │
   ▼
Structured Output Parser
   │
   ▼
Candidate Info Extractor
   │
   ▼
Append Results → Google Sheets

🗂️ Repository Structure
.
├── README.md
├── Resume Screening.json   # n8n workflow (import directly)
└── assets/                # (optional) screenshots / diagrams

🔧 Workflow Breakdown (Node-Wise)
1️⃣ Resume Collection

Trigger: Gmail Trigger

Automatically listens for new emails with attachments

Downloads resume files securely

2️⃣ File Type Detection

Uses a Switch node to route:

.docx

.pdf

.txt

3️⃣ Resume Normalization

DOCX → Converted to Google Docs → Downloaded as PDF

PDF/TXT → Direct text extraction

All formats end as clean resume text

4️⃣ Job Description Ingestion

Job description pulled from Google Drive

Parsed and injected into AI prompt

5️⃣ AI Recruiter Agent

Powered by OpenAI (o4-mini) via LangChain agent.

Produces:

Candidate strengths & weaknesses

Risk factor (Low / Medium / High)

Reward factor + time horizon

Dimension-wise scores:

Skills match

Experience relevance

Role & domain fit

Seniority alignment

Red flags

Weighted score calculation

Final fit rating (0–10)

6️⃣ Structured Output Parsing

Enforces strict JSON schema

Guarantees clean downstream automation

7️⃣ Candidate Metadata Extraction

Extracts:

First Name

Last Name

Email Address

8️⃣ Reporting & Storage

Resume stored in Google Drive

Full evaluation appended to Google Sheets

Each row = one candidate evaluation

📊 Output Schema (Simplified)
{
  "candidate_strengths": [],
  "candidate_weaknesses": [],
  "risk_factor": { "score": "", "explanation": "" },
  "reward_factor": { "score": "", "explanation": "", "time_horizon_fit": "" },
  "overall_fit_rating": 0,
  "dimension_wise_scoring": {},
  "weighted_score_calculation": {},
  "justification_for_rating": ""
}

⚙️ Setup Instructions
Prerequisites

n8n (cloud or self-hosted)

Google Workspace account

OpenAI API key

Step-by-Step

Import Workflow

n8n → Import → Upload Resume Screening.json

Configure Credentials

Gmail OAuth

Google Drive OAuth

Google Sheets OAuth

OpenAI API

Update References

Replace:

Google Drive folder ID (resume storage)

Job Description file ID

Google Sheet ID

Test the Workflow

Send an email with a resume attachment

Verify data appears in Google Sheets

🧠 Design Philosophy

Format-agnostic ingestion

Deterministic AI outputs (strict schema)

Human-readable recruiter reasoning

Audit-friendly scoring

Low-code, highly extensible

🚀 Use Cases

Startup hiring automation

Recruitment agencies

Internal HR screening

AI-assisted talent pipelines

🔮 Future Enhancements

Candidate ranking across multiple resumes

ATS integrations (Greenhouse, Lever)

Slack / Email decision notifications

Bias & fairness checks

Multi-JD comparison
