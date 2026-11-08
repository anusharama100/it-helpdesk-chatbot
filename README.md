# IT Helpdesk Chatbot

A React chat interface backed by a Python service for common IT questions. It looks up relevant entries in a CSV knowledge base and uses those entries to generate an answer with source references.

**React · TypeScript · FastAPI · LangChain · OpenAI · ChromaDB**

## How it works

```text
React chat → FastAPI → ChromaDB retrieval → model response + sources
```

Quick actions start common troubleshooting requests. Conversations stay in memory. The escalation flow returns a sample ticket ID; it does not send a ticket to a support team.

## Run locally

Use Python 3.11 and Node.js with npm. Model requests require your own OpenAI API key and can incur usage charges.

```sh
git clone https://github.com/anusharama100/it-helpdesk-chatbot.git
cd it-helpdesk-chatbot
python3 -m venv .venv
source .venv/bin/activate
pip install -r backend/requirements.txt
cd backend
cp .env_example .env
```

Add your key to `backend/.env`, then run `python main.py` from `backend/`. Keep the key on the backend.

In a second terminal, from the repository root:

```sh
cd frontend
npm install
npm run dev
```

Open [localhost:5173](http://localhost:5173). The API uses [localhost:8000](http://localhost:8000), with endpoint documentation at [/docs](http://localhost:8000/docs). Set `VITE_API_BASE_URL` if the backend runs elsewhere.

## Try it

Ask “My Zoom audio isn't working,” then explain what you have already tried. Inspect the source references and compare the answer with a networking or password-reset quick action.

## Code to start with

- `backend/data/it_knowledge.csv` — knowledge source.
- `backend/knowledge_base.py` — retrieval setup.
- `backend/chat_engine.py` — conversation logic.
- `frontend/` — chat interface and API client.

## Project status

A development prototype with JSON responses, in-memory conversations, and simulated tickets. Authentication, durable history, real ticket delivery, and answer-quality evaluation remain open work.

See [SETUP.md](SETUP.md) for configuration and Docker notes.
