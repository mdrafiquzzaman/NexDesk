# NexDesk — AI-Powered IT Service Management Platform

**NexDesk** is a full-stack, AI-native IT service management platform built to modernize the help desk experience. It combines the structured workflows of enterprise tools like ServiceNow with the intelligence of large language models — automating ticket triage, classification, and resolution suggestions in real time.

> Built by Md Rafiquzzaman · [Live Demo](#) · [API Docs](https://nexdesk-api.onrender.com/docs)

---

## Features

### AI-Powered Ticket Intelligence
- **Auto-classification** — AI reads plain-English issue descriptions and assigns category, priority, and affected system automatically
- **Resolution Copilot** — AI suggests step-by-step resolution steps before a technician even opens the ticket, drawing from a structured knowledge base
- **Conversational Intake** — Users can submit tickets through a natural chat interface instead of a form
- **Pattern Detection** — Identifies spikes in ticket categories (e.g., "12 VPN tickets this week") and surfaces alerts on the dashboard

### Full ITSM Workflow
- Ticket lifecycle management: Open → In Progress → Resolved → Closed
- Ticket activity log — every status change, assignment, and AI action is tracked
- Technician assignment and resolution note documentation
- Ticket filtering by status, priority, and category

### Analytics Dashboard
- Ticket volume over time (7-day trend)
- Breakdown by category and priority
- Average resolution time
- Real-time pattern/anomaly alerts

### Technical Architecture
- **Frontend**: React 18, React Router, Recharts, Lucide Icons, Vite
- **Backend**: Python FastAPI, SQLite, Anthropic Claude API (claude-sonnet-4)
- **Deployment**: Render (backend API + frontend static site)
- **AI**: Anthropic Claude API for classification, resolution generation, and conversational intake

---

## Getting Started

### Prerequisites
- Python 3.10+
- Node.js 18+
- An [Anthropic API key](https://console.anthropic.com/) (free tier works)

### 1. Clone the repo
```bash
git clone https://github.com/mdrafiquzzaman/nexdesk.git
cd nexdesk
```

### 2. Start the backend
```bash
cd backend
pip install -r requirements.txt
export ANTHROPIC_API_KEY=your_key_here
uvicorn main:app --reload
```
Backend runs at `http://localhost:8000`
Interactive API docs at `http://localhost:8000/docs`

### 3. Start the frontend
```bash
cd frontend
cp .env.example .env
npm install --legacy-peer-deps
npm run dev
```
Frontend runs at `http://localhost:5173`

> **Note:** If `ANTHROPIC_API_KEY` is not set, NexDesk falls back to a rule-based classifier and knowledge base — fully functional without an API key.

---

## Deploying to Render

NexDesk ships with a `render.yaml` blueprint for one-click deployment.

### Steps
1. Push this repo to GitHub
2. Go to [render.com](https://render.com) → New → Blueprint
3. Connect your GitHub repo
4. Render will create both services automatically
5. Set `ANTHROPIC_API_KEY` in the `nexdesk-api` service's environment variables
6. Update `VITE_API_URL` in `nexdesk-frontend` to your backend URL

### Manual deploy (without blueprint)

**Backend (Web Service)**
- Runtime: Python 3
- Build command: `pip install -r requirements.txt`
- Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
- Add a persistent disk at `/data` for the SQLite database
- Environment variable: `ANTHROPIC_API_KEY`

**Frontend (Static Site)**
- Build command: `npm install --legacy-peer-deps && npm run build`
- Publish directory: `dist`
- Environment variable: `VITE_API_URL=https://your-backend.onrender.com/api`
- Add rewrite rule: `/* → /index.html`

---

## Project Structure

```
nexdesk/
├── backend/
│   ├── main.py                 # FastAPI app entry point
│   ├── database.py             # SQLite setup + knowledge base seed
│   ├── requirements.txt
│   ├── routers/
│   │   ├── tickets.py          # CRUD endpoints
│   │   ├── ai.py               # AI classification + chat endpoints
│   │   └── analytics.py        # Dashboard metrics
│   └── services/
│       ├── ai_service.py       # Claude API integration
│       └── ticket_service.py   # Ticket utilities
├── frontend/
│   ├── src/
│   │   ├── App.jsx             # Routing
│   │   ├── pages/
│   │   │   ├── Dashboard.jsx   # Main overview
│   │   │   ├── Tickets.jsx     # Ticket list + filters
│   │   │   ├── TicketDetail.jsx # Full ticket view + edit
│   │   │   ├── NewTicket.jsx   # Ticket submission form
│   │   │   ├── ChatIntake.jsx  # Conversational AI intake
│   │   │   └── Analytics.jsx   # Charts + metrics
│   │   ├── components/
│   │   │   ├── Sidebar.jsx
│   │   │   ├── StatCard.jsx
│   │   │   └── Toast.jsx
│   │   └── lib/
│   │       ├── api.js          # Axios API layer
│   │       └── utils.js        # Helpers + constants
│   └── vite.config.js
├── render.yaml                 # Render deployment blueprint
└── README.md
```

---

## API Reference

Full interactive docs available at `/docs` when running the backend.

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/tickets/` | List all tickets (filterable) |
| POST | `/api/tickets/` | Create ticket (triggers AI classification) |
| GET | `/api/tickets/{id}` | Get ticket + event log |
| PATCH | `/api/tickets/{id}` | Update status, priority, assignee, notes |
| DELETE | `/api/tickets/{id}` | Delete ticket |
| POST | `/api/ai/chat` | Conversational ticket intake |
| POST | `/api/ai/classify` | Classify a description |
| GET | `/api/analytics/summary` | Dashboard metrics + pattern alerts |

---

## AI Fallback Mode

NexDesk works without an Anthropic API key using a rule-based fallback:
- Keyword-based category detection (password, vpn, printer, etc.)
- Urgency-based priority assignment
- Knowledge base resolution lookup by category

This means the app is fully demonstrable without any API costs.

---

## License

MIT License — free to use, modify, and deploy.

---

*NexDesk was built as a portfolio project demonstrating full-stack development, REST API design, AI/LLM integration, and modern IT service management workflows.*
