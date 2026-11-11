# TraceLens

A locally runnable human-centred AI-agent oversight research prototype. This is a synthetic benchmark demo; it does not contain real user-study data. Visible rationales are agent-provided explanations and should not be interpreted as faithful internal reasoning.

## Run locally

Requirements: Python 3.10+ and Node.js 18+.

Terminal 1:

```powershell
cd backend
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

Terminal 2:

```powershell
cd frontend
npm install
npm run dev
```

Open http://localhost:5173. The API creates `backend/tracelens.sqlite3` and `backend/data/benchmark.json` on first start.

## Included

- 30 deterministic synthetic trajectories with suspicious, legitimate, and insufficient-evidence labels.
- Rationale-only, action-only and combined simulated heuristic monitors with trace references and explicit uncertainty.
- Raw Logs, Basic Monitor and TraceLens presentation conditions.
- Review workflow for decision, confidence, elapsed time, evidence consulted, justification, monitor agreement, approve/pause/escalate; persisted in SQLite.
- Pseudonymous participant assignment across conditions, stored to SQLite with stable assignment and shuffled scenario order.
- Benchmark monitor metrics; human metrics are calculated only from recorded reviews. Calibration and comprehension are not claimed without adequate participant data / human coding.

## Monitor / API configuration

No API key is required. With no `OPENAI_API_KEY`, the app uses deterministic simulated outputs. With a key, it calls an OpenAI-compatible `/chat/completions` endpoint using `OPENAI_BASE_URL` (default `https://api.openai.com/v1`) and `OPENAI_MODEL` (default `gpt-4o-mini`). The key is read only from the environment. If a request fails, the endpoint falls back to the labeled deterministic monitor. Configure these environment variables before starting the API.

API routes: `GET /api/health`, `GET /api/trajectories`, `GET /api/trajectories/{id}/monitor`, `GET /api/evaluation`, `POST /api/assign`, `POST /api/reviews`.


## One-click deployment (Render)

This repository includes a Render Blueprint and a Docker build that serves the React app and FastAPI API on one origin. This is configured as a free demo deployment. SQLite and generated fixture files use the service filesystem, which is ephemeral on Render Free.

1. Upload the contents of this `tracelens` folder to a GitHub repository.
2. In this README, replace `pratbat05/tracelens` in the Deploy button link with your repository path.
3. Open the README on GitHub and click **Deploy to Render**. Review the Blueprint and approve it. The Blueprint uses Render's free web-service plan and does not request a persistent disk. Render free services have an ephemeral filesystem, so SQLite reviews and generated fixture files can be cleared on restart, spin-down, or redeploy. See [Render free-tier limits](https://render.com/docs/free).
4. After deployment, the app URL serves the frontend and API together. It works in DEMO MODE without an API key. To enable a model endpoint, add `OPENAI_API_KEY` in Render's environment settings (and optionally change `OPENAI_BASE_URL` / `OPENAI_MODEL`). Never commit the key.

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/pratbat05/tracelens)

Render's deploy button uses the repository URL in that link to discover the Blueprint in `render.yaml`. See [Render deploy button instructions](https://render.com/docs/deploy-to-render).


