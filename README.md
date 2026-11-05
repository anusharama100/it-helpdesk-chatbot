# IT Helpdesk Chatbot

A support-chat prototype for common workplace IT questions, including Wi-Fi, password resets, software installation, and audio/video troubleshooting. It retrieves relevant entries from a CSV knowledge base and uses them to generate a conversational answer.

The project connects a React chat interface to a Python API, with source references and an escalation path when a conversation needs human help.

## How a request moves through the system

```text
React chat -> FastAPI -> knowledge retrieval in ChromaDB
                       -> LangChain + OpenAI -> JSON answer and sources
```

The backend maintains conversation context in memory. Quick-action buttons start common troubleshooting flows. Escalation returns a simulated ticket ID; it does not create a ticket in Jira or ServiceNow, send email, or notify a support team.

## Run locally

Use Python 3.11 and Node.js with npm. The backend requires an OpenAI API key and network access for model services.

```sh
git clone https://github.com/anusharama100/it-helpdesk-chatbot.git
cd it-helpdesk-chatbot
python3 -m venv .venv
source .venv/bin/activate
pip install -r backend/requirements.txt
cd backend
cp .env_example .env
```

Add your own key to `backend/.env`, then start the API from that directory:

```sh
python main.py
```

In a second terminal, from the repository root:

```sh
cd frontend
npm install
npm run dev
```

Open [localhost:5173](http://localhost:5173). The API defaults to [localhost:8000](http://localhost:8000), with interactive documentation at [/docs](http://localhost:8000/docs). The frontend defaults to that API address; `VITE_API_BASE_URL` can override it.

## Try it

Ask “My Zoom audio isn't working,” follow up with what you have already tried, and inspect the returned source references. Compare that with a quick action for a password or networking issue.

| Endpoint | Purpose |
| --- | --- |
| `POST /chat` | Submit a message and optional conversation ID |
| `GET /quick-actions` | List common support tasks |
| `POST /quick-action/{action_id}` | Start a predefined troubleshooting request |
| `POST /ticket` | Simulate escalation |
| `GET /conversation/{id}/history` | Read in-memory conversation history |
| `DELETE /conversation/{id}` | Clear a conversation |
| `GET /health` | Inspect initialized services |

Responses arrive as JSON; the current chat endpoint does not stream tokens. The analytics endpoint logs events rather than serving an analytics dashboard.

## Implementation

- **Backend:** FastAPI, Pydantic, LangChain, OpenAI, and ChromaDB.
- **Frontend:** React, TypeScript, Vite, Tailwind CSS, and Axios.
- **Knowledge source:** `backend/data/it_knowledge.csv`.
- **Retrieval setup:** `backend/knowledge_base.py`.
- **Conversation logic:** `backend/chat_engine.py`.

Backend configuration includes `MODEL_NAME`, `TEMPERATURE`, `MAX_TOKENS`, `CORS_ORIGINS`, and `CHROMA_PERSIST_DIRECTORY`. Keep credentials on the backend.

## Scope and follow-up work

This is a development prototype with in-memory conversations and simulated ticket handling. Production work includes authentication, durable storage, real ticket delivery, and evaluation of answer quality against the knowledge base. API usage may incur provider charges.

Additional setup notes are in [SETUP.md](SETUP.md), and the repository includes Docker configuration for further development.
