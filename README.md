# Agentic Healthcare Assistant

A virtual medical assistant that reads a request in plain English and decides which **tool** to use: patient records, history retrieval, AI-written summaries, appointment booking, or disease information. Every action is logged, and a metrics view shows how the assistant is being used.

Built with Python, the OpenAI API (`gpt-4o-mini`) and Gradio.

> **Educational prototype.** It uses in-memory sample data, is not a medical device, and does not provide medical advice.

## Why

Healthcare work spans scheduling, record keeping, and finding reliable information, often across separate tools. This project shows how a single agent can sit in front of several tools and route each request to the right one, with safety rules built into every LLM prompt.

## How it works

```
User request + patient name
        │
        ▼
Healthcare agent (intent routing)
        │
        ├── "history" / "record" / "details" ──► Patient history tool
        ├── "summarize" / "summary" ───────────► AI summary tool (LLM)
        ├── "appointment" / "book" / "doctor" ─► Appointment booking
        ├── "disease" / "symptom" / "treatment" ► Disease information tool (LLM)
        └── anything else ─────────────────────► General safe response (LLM)
        │
        ▼
Response + activity log
```

| Tool | What it does | Uses an LLM? |
|---|---|---|
| **Patient record** | Add or update a patient's name, age, condition, and history | No |
| **Patient history** | Retrieve a stored record | No |
| **AI summary** | Summarize a patient's history in plain language | Yes |
| **Appointment booking** | Book by doctor type and date, and list bookings | No |
| **Disease information** | Explain a condition: what it is, symptoms, general treatment, when to see a doctor | Yes |
| **Agent logs and metrics** | Timestamped log of every action, plus usage counts | No |

**Safety rules in every prompt:** no diagnosis, no prescriptions, and a reminder to consult a qualified doctor.

## Features

| Tab | What it does |
|---|---|
| **API Key** | Enter your OpenAI key at runtime (skip if `OPENAI_API_KEY` is set) |
| **Chat with Healthcare Agent** | Ask in natural language; the agent picks the tool |
| **Add Patient Record** | Create or update a record |
| **View Patient History** | Retrieve a record |
| **Summarize Patient History** | AI summary in simple language |
| **Book Appointment** | Choose a doctor type and date |
| **View Appointments** | List all bookings |
| **Disease Information** | Educational overview of a condition |
| **Agent Logs** | Every action with a timestamp |
| **Evaluation Metrics** | Counts of records, appointments, and logged actions |

## Tech stack

- **Python**
- **OpenAI API**: `gpt-4o-mini` via the official Python SDK
- **Gradio**: multi-tab web interface
- **Google Colab / Jupyter**: runtime

## Getting started

1. Open `Healthcare-Assistant.ipynb` in **Google Colab** or Jupyter.
2. Provide your OpenAI API key in **one** of these ways:
   - Set the `OPENAI_API_KEY` environment variable before starting, **or**
   - Paste it in the app's **API Key** tab after launch.
3. Run the cell. Dependencies (`gradio`, `openai`) install automatically.
4. Open the Gradio link, add a patient record, then try the chat, for example:
   - "Please summarize my father's kidney disease history"
   - "What are the symptoms of chronic kidney disease?"

The API key is never written into the notebook or this repository.

## Limitations and next steps

- **LLM-based routing:** replace keyword matching with OpenAI function calling, so the model chooses tools and extracts arguments itself
- **Trusted sources:** add retrieval (RAG) over vetted sources such as MedlinePlus or WHO for disease information, with citations
- **Persistent storage:** move records and appointments from in-memory Python objects to a database
- **Real scheduling:** check doctor availability and prevent double-booking
- **Evaluation:** add test queries with expected tools to measure routing accuracy
- **Privacy:** add authentication and access control before handling any real patient data

## Author

**Ashish Mahashabde**: engineer and AI builder
