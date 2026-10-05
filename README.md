# NOVEXA NOWCAST

## Explainable Convective Intelligence for Maharashtra

NOVEXA NOWCAST is an end-to-end AI/ML-enabled decision-support platform for
convective weather nowcasting, multi-hazard assessment, district-level risk
intelligence, and early-warning operations across Maharashtra.

The platform combines a modern operational frontend with a FastAPI backend,
an explainable inference pipeline, continuously evolving nowcast state,
storm-cell tracking, forecast movement, hazard classification, validation,
telemetry, and deployment-ready service integration.

## Why NOVEXA NOWCAST

Weather information becomes valuable when it leads to a timely decision.
NOVEXA NOWCAST is designed to help state and district operators answer:

- What is developing now?
- Which cells and hazards require attention?
- Which districts and assets are exposed?
- What is the expected movement over the next few minutes?
- How reliable is the current analysis?
- What action should an operator take next?

The product is designed around explainability, operational clarity, data
quality, forecast confidence, and a clear connection between detection and
response.

## Implemented Capabilities

### AI/ML and analytical intelligence

- Modular feature-vector construction for convective analysis.
- Explainable weighted inference pipeline with feature contributions.
- Model registry and AI pipeline readiness status.
- Storm-cell detection, identification, tracking, and evolution.
- Cell motion, intensity trend, growth/decay, and forecast lead times.
- Multi-hazard assessment for lightning, heavy rain, wind, and hail.
- Confidence scoring and source-agreement indicators.
- District and regional risk aggregation.
- Alert generation with severity, rationale, affected geography, and lead time.
- Scenario parameters for controlled sensitivity analysis.
- Repeatability, track continuity, forecast, hazard, and spatial validation.
- Performance and data-quality telemetry for operational monitoring.

### Operational Command Center

- Maharashtra-wide interactive MapLibre map.
- Live backend connection status and WebSocket state.
- Active storm-cell and hazard visualization.
- Forecast tracks, movement vectors, risk zones, and district overlays.
- Alert and hazard prioritization.
- Analysis timestamp, system clock, sequence, and snapshot identity.
- Source quality, confidence, and backend readiness indicators.
- Graceful recovery and last-known-state behavior during connection loss.

### Scenario Lab

- Scenario selection and parameter controls.
- Multi-cell storm evolution and replay progression.
- Forecast horizon and lead-time exploration.
- Hazard sensitivity analysis.
- Scenario comparison and validation visibility.
- Safe separation between operational monitoring and analysis workflows.

### Engineering and reliability

- Versioned REST API under `/api` and `/api/v1`.
- WebSocket stream at `/api/ws/nowcast`.
- Health and readiness endpoints.
- Runtime snapshot validation with Zod on the frontend.
- Sequence and stale-snapshot protection.
- Deterministic backend state generation for reproducible analysis.
- Responsive React interface for desktop and operational displays.
- Render deployment configuration for the backend.
- Vercel deployment configuration for the frontend.
- Automated backend tests, TypeScript validation, and production builds.

## System Architecture

```text
┌─────────────────────────────────────────────────────────────┐
│                    NOVEXA NOWCAST                          │
├──────────────────────────────┬──────────────────────────────┤
│ React + TypeScript frontend  │ FastAPI analytical backend    │
│                              │                              │
│ Command Center               │ State and scenario engine    │
│ Scenario Lab                 │ AI/ML inference pipeline      │
│ Operations Map               │ Storm tracking and forecast   │
│ Performance and validation   │ Hazard and alert rules        │
│ Backend/WebSocket provider   │ Health, telemetry, validation  │
└───────────────┬──────────────┴──────────────┬───────────────┘
                │ REST + WebSocket             │
                └──────────────┬──────────────┘
                               │
                    Render backend service
                               │
                    Vercel frontend deployment
```

### Technology stack

| Layer | Technology |
|---|---|
| Frontend | React 19, TypeScript, Vite 8 |
| Styling | Tailwind CSS v4 |
| Mapping | MapLibre GL |
| State | Zustand |
| Runtime validation | Zod |
| Backend | Python, FastAPI, Uvicorn |
| Analytical computing | NumPy, Shapely |
| API schemas | Pydantic v2 |
| Transport | REST API and WebSocket |
| Frontend hosting | Vercel |
| Backend hosting | Render |

## Repository Structure

```text
.
├── backend/
│   ├── app/
│   │   ├── ai_pipeline.py       # AI/ML feature and inference pipeline
│   │   ├── domain.py            # Maharashtra analysis domain
│   │   ├── engine.py            # nowcast state and storm evolution engine
│   │   ├── main.py              # FastAPI routes and WebSocket endpoint
│   │   ├── schemas.py            # API and analytical data contracts
│   │   └── validation.py         # validation and telemetry calculations
│   ├── tests/
│   ├── requirements.txt
│   └── README.md
├── frontend/
│   ├── src/
│   │   ├── components/           # shell, map, clocks, panels, controls
│   │   ├── lib/                  # API, health, schemas, utilities
│   │   ├── pages/                # command center and analysis pages
│   │   ├── providers/            # backend and local data providers
│   │   ├── store/                # application state and mode selection
│   │   └── types/                # shared frontend types
│   ├── package.json
│   └── vercel.json
├── docs/
│   ├── API-CONTRACT.md
│   ├── SYSTEM-STATUS-CONTRACT.md
│   └── final-system-audit.md
├── render.yaml
└── README.md
```

## Local Development

### Prerequisites

- Python 3.12 or newer
- Node.js 20 or newer
- pnpm

### Start the backend

```powershell
cd backend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```

Backend URLs:

```text
API:       http://127.0.0.1:8000
Swagger:   http://127.0.0.1:8000/docs
Health:    http://127.0.0.1:8000/api/v1/health
Readiness: http://127.0.0.1:8000/api/v1/ready
WebSocket: ws://127.0.0.1:8000/api/ws/nowcast
```

### Start the frontend

In a second terminal:

```powershell
cd frontend
pnpm install
pnpm dev
```

Open:

```text
http://localhost:8443
```

For local split-origin development, create `frontend/.env.local`:

```env
VITE_API_URL=http://127.0.0.1:8000/api
```

The frontend automatically derives REST and secure/insecure WebSocket URLs
from `VITE_API_URL`.

## API Surface

### Platform status

```text
GET /api/v1/health
GET /api/v1/ready
GET /api/v1/ai/status
GET /api/v1/models
GET /api/v1/data-quality
GET /api/v1/performance
GET /api/v1/validation
```

### Nowcast and scenarios

```text
GET  /api/v1/nowcast/current
GET  /api/v1/storms
GET  /api/v1/hazards
GET  /api/v1/alerts
GET  /api/v1/scenarios
POST /api/v1/scenarios/{scenario_id}/run
GET  /api/v1/map/state
WS   /api/ws/nowcast
```

The complete request and response contracts are documented in
[docs/API-CONTRACT.md](docs/API-CONTRACT.md).

## Connecting the Deployed Services

NOVEXA NOWCAST is designed to run as two connected deployments:

```text
Vercel frontend  ──HTTPS/WSS──>  Render FastAPI backend
```

### Render backend

Create a Render Web Service using the repository and configure:

```text
Root Directory:   backend
Runtime:          Python
Build Command:    pip install -r requirements.txt
Start Command:    uvicorn app.main:app --host 0.0.0.0 --port $PORT
Health Check:     /api/v1/health
```

The same configuration is available in [render.yaml](render.yaml).

Add this Render environment variable after creating the frontend:

```text
CORS_ORIGINS=https://YOUR-FRONTEND.vercel.app
```

Multiple origins may be comma-separated.

### Vercel frontend

Create a Vercel project from the repository and set:

```text
Root Directory:   frontend
Framework:        Vite
Install Command:  pnpm install --frozen-lockfile
Build Command:    pnpm run build
Output Directory: dist
```

Add this Vercel environment variable:

```text
VITE_API_URL=https://YOUR-BACKEND.onrender.com/api
```

Enable it for Production and Preview environments as required, then redeploy
the frontend. The value must include `/api` and must not end with a slash.

### Deployment verification

1. Open the Render health URL and confirm HTTP 200:
   `https://YOUR-BACKEND.onrender.com/api/v1/health`
2. Confirm readiness:
   `https://YOUR-BACKEND.onrender.com/api/v1/ready`
3. Set `CORS_ORIGINS` to the exact Vercel origin.
4. Set `VITE_API_URL` to the Render URL plus `/api`.
5. Redeploy both services.
6. In the browser Network panel, verify REST requests use the Render domain.
7. Verify the WebSocket connects through:
   `wss://YOUR-BACKEND.onrender.com/api/ws/nowcast`

## Testing and Quality Checks

Run the backend tests:

```powershell
cd backend
python -m pytest -q
```

Check the frontend types and production build:

```powershell
cd frontend
pnpm exec tsc --noEmit
pnpm run build
```

Check repository whitespace:

```powershell
git diff --check
```

## Operational Demo Flow

For a complete product demonstration:

1. Open the Command Center and verify backend health and readiness.
2. Review active cells, movement vectors, forecast lead time, and hazards.
3. Open the district risk view and inspect affected geography.
4. Open the explainability panel to review inference drivers and confidence.
5. Use Scenario Lab to test a changing convective situation.
6. Compare forecast, hazard, and validation metrics.
7. Return to the Command Center and demonstrate live snapshot progression.
8. Show backend recovery behavior and restored state synchronization.

## Product Direction

NOVEXA NOWCAST is built as a foundation for operational weather intelligence:
an explainable analytical layer, a resilient state and API contract, and an
operator-focused interface that can evolve with additional observation
providers, historical evaluation datasets, alert delivery channels, and
district response integrations.

The objective is simple: convert complex convective intelligence into clear,
timely, and actionable decisions for Maharashtra.
