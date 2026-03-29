# AI Travel Planner

A full-stack AI-powered travel planning application that uses natural language processing to understand travel preferences and generate personalized day-by-day itineraries.

**Live Demo:** [ai-travel-planner-delta-seven.vercel.app](https://ai-travel-planner-delta-seven.vercel.app/)

---

## Features

- **AI Chat Interface** — Conversational trip planning with smart follow-up prompts
- **Intent Classification** — Zero-shot NLI via facebook/bart-large-mnli maps free-text input to travel categories (beach, mountains, city, culture, food)
- **Destination Recommendations** — Curated catalog filtered by category, best travel months, and user preferences
- **Itinerary Generation** — Day-by-day itinerary builder with activities, dining, and logistics using DistilGPT-2
- **Multi-Agent Architecture** — Orchestrator pattern coordinates recommender, itinerary builder, and optional POI provider
- **Responsive UI** — Mobile-first React frontend with Tailwind CSS and smooth animations

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 19, TypeScript, Vite, Tailwind CSS |
| Backend | Python, FastAPI, Uvicorn |
| AI/ML | Hugging Face Transformers, facebook/bart-large-mnli, DistilGPT-2 |
| Deployment | Vercel (frontend) |

## Architecture

```
User Input
    │
    ▼
┌──────────┐    ┌──────────────┐    ┌───────────────────┐
│  React   │───▶│   FastAPI    │───▶│  Manager Agent     │
│  Client  │◀───│   Backend    │◀───│  (Orchestrator)    │
└──────────┘    └──────────────┘    └───────┬───────────┘
                                      ┌─────┼─────┐
                                      ▼     ▼     ▼
                                 Recommender  Itinerary  POI
                                  Agent       Builder   Provider
```

- **Manager** — Coordinates multi-turn conversation and routes between agents
- **Recommender** — Zero-shot intent classification using facebook/bart-large-mnli
- **Itinerary Builder** — Generates day-by-day plans from destination/POI data; falls back to templates if ML is unavailable
- **POI Provider** — Optional external POI lookups via RapidAPI TripAdvisor (disabled by default)

## Getting Started

### Prerequisites

- Node.js 18+ and Yarn
- Python 3.10+
- Hugging Face API token (optional — falls back to local inference)

### Frontend

```bash
cd AI-Travel-Planner-Frontend
yarn install
yarn dev
```

Runs at `http://localhost:5173` by default.

### Backend

```bash
cd AI-Travel-Planner-Backend
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scriptsctivate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### Environment Variables

Create a `.env` file in the backend directory:

```env
HF_API_TOKEN=your_huggingface_token    # Optional: falls back to local inference
RECOMMENDER_MODEL=facebook/bart-large-mnli
ITINERARY_MODEL=distilgpt2
POI_ENABLED=false
```

Frontend `.env.development`:

```env
VITE_API_BASE=http://localhost:8000
```

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/recommendations/classify-intent` | Classify travel intent from text |
| POST | `/api/itinerary/generate` | Generate day-by-day itinerary |
| GET | `/api/recommendations/destinations` | Filter destinations by tag/month |
| POST | `/api/plan` | Single-call orchestrated planning flow |

## Project Structure

```
AI-Travel-Planner/
├── AI-Travel-Planner-Frontend/
│   └── src/
│       ├── api/client.ts          # Typed API wrapper
│       ├── component/Home.tsx     # Main chat UI
│       └── component/smaller-component/
│           └── PlanDestination/   # Destination selection
├── AI-Travel-Planner-Backend/
│   └── app/
│       ├── agents/
│       │   ├── manager.py         # Orchestrator
│       │   ├── recommender.py     # Intent classification
│       │   ├── itinerary_builder.py
│       │   └── poi_provider.py    # Optional POI lookups
│       ├── data/
│       │   ├── destinations.json  # Destination catalog
│       │   └── places.json        # POI/activity data
│       ├── main.py
│       └── schemas.py
└── README.md
```

## License

This project is for educational purposes.