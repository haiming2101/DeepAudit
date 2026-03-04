# DeepAudit Day 1 (60-Minute) Project Walkthrough

This guide is designed for your **first hour** with DeepAudit so you can understand the architecture, run the product, and complete one end-to-end audit.

---

## Agenda at a Glance

| Time | Goal | What you'll do |
|---|---|---|
| 0-10 min | Understand what DeepAudit is | Read key docs and map core modules |
| 10-25 min | Run DeepAudit locally | Start services with Docker Compose |
| 25-40 min | Execute your first audit | Create/import a project and launch an audit task |
| 40-50 min | Read outputs like a security engineer | Inspect findings, verification status, and report export |
| 50-60 min | Build your mental model for contribution | Learn backend/frontend hotspots and next steps |

---

## 0-10 min: Orientation and System Mental Model

1. Read these in order:
   - `README.md` (or `README_EN.md`) for product positioning and quick start.
   - `docs/ARCHITECTURE.md` for service boundaries.
   - `docs/AGENT_AUDIT.md` for Agent-mode workflow.

2. Understand the core idea:
   - DeepAudit uses a **Multi-Agent pipeline** (Orchestrator -> Recon -> Analysis -> Verification).
   - The Verification phase runs PoC logic in a **sandbox container** to reduce false positives.

3. Keep this directory map in mind:
   - `backend/` -> FastAPI APIs, audit orchestration, LLM + RAG services.
   - `frontend/` -> React + TypeScript UI for task creation, logs, reports.
   - `docker/` -> sandbox and deployment plumbing.
   - `docs/` -> operational and architecture references.

---

## 10-25 min: Bring Up a Local Environment

### Fast path (recommended)

From repo root:

```bash
docker compose up -d
docker compose ps
```

Then open:

- Frontend: `http://localhost:3000`
- Backend API docs: `http://localhost:8000/docs`

### If this is your first run

- Initial startup can take a few minutes (image pull/build, DB init, sandbox prep).
- If a service is unhealthy, inspect logs:

```bash
docker compose logs -f backend
docker compose logs -f frontend
docker compose logs -f db
```

### Minimum configuration check

- Confirm `backend/.env` exists (copied from `backend/env.example`).
- Ensure at least one LLM provider is configured (API key/base URL/model).
- If your goal is offline/private use, review `docs/LLM_PROVIDERS.md` for local model options.

---

## 25-40 min: Run Your First Audit Task

1. Open the web app and login/register.
2. Create a new project (or import from Git platform if already configured).
3. Go to **Agent Audit** and create a task:
   - Select project scope.
   - Choose model/provider.
   - Keep default options for first run (optimize for successful baseline).
4. Start the task and watch live logs.

What to look for in logs:

- **Recon**: framework/endpoints/entry points discovered.
- **Analysis**: candidate vulnerabilities and reasoning.
- **Verification**: sandbox PoC attempts, retries, success/failure evidence.
- **Orchestrator**: final aggregation and deduped conclusions.

---

## 40-50 min: Interpret Results and Export Report

1. Open task detail and review vulnerability list.
2. Prioritize by:
   - verified exploitable findings,
   - severity,
   - confidence and evidence quality.
3. Export a report (PDF/Markdown/JSON depending on your workflow).
4. Validate that each high-risk issue has:
   - clear affected file/endpoint,
   - exploit path or PoC trace,
   - remediation suggestion.

This step is where DeepAudit is strongest: combining semantic analysis + verification instead of only static pattern matching.

---

## 50-60 min: Contributor Onboarding Map

If you want to start contributing right after Day 1:

### Backend hotspots

- `backend/app/services/agent/agents/` -> agent roles and orchestration behavior.
- `backend/app/services/agent/tools/` -> tool-calling capabilities (file access, sandbox, reporting).
- `backend/app/services/llm/` -> provider adapters and model integration.
- `backend/app/services/rag/` -> indexing/retrieval pipeline.

### Frontend hotspots

- `frontend/src/pages/AgentAudit/` -> audit UX shell.
- `frontend/src/components/agent/` -> task dialogs and configuration controls.
- `frontend/src/components/audit/` -> non-agent audit workflows.

### Immediate next tasks (Day 2+)

1. Run backend tests and inspect failures/warnings.
2. Trace one full task lifecycle from API endpoint -> DB model -> UI rendering.
3. Add one small improvement (UI hint, log enrichment, provider config validation).
4. Document what you changed in `docs/` to keep onboarding smooth.

---

## Optional 5-Minute Checklist (after the hour)

- [ ] I can start DeepAudit locally with one command.
- [ ] I know where Agent orchestration logic lives.
- [ ] I ran at least one audit task end-to-end.
- [ ] I can explain the difference between "detected" and "verified" findings.
- [ ] I know which module I want to modify first.

You're now set up for productive contribution and realistic security validation workflows.
