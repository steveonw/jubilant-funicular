# PolicyTrace

> **Traceable AI for policy and public sentiment analysis**

**Team:** Steveon Walker and Dreshawn Young  
**Current contest build:** build 27  
**Microsoft Foundry model:** `gpt-5-mini`

PolicyTrace is an evidence-grounded policy analysis system that helps analysts turn complex regulations, public comments, and related reporting into concise leadership briefings while preserving a traceable evidence trail for every important claim.

The core idea is simple:

> **No important AI claim without evidence. No evidence without a support check. No final report without human review.**

## Contest materials

- **Presentation:** [PolicyTrace_Contest_Presentation.pptx](./PolicyTrace_Contest_Presentation.pptx)
- **Microsoft Foundry setup:** [MICROSOFT_FOUNDRY_SETUP.md](./MICROSOFT_FOUNDRY_SETUP.md)
- **Current contest handoff:** [docs/CONTEST_HANDOFF_CURRENT.md](./docs/CONTEST_HANDOFF_CURRENT.md)
- **Build 27 handoff:** [docs/CONTEST_HANDOFF_PHASE_9_BUILD_27.md](./docs/CONTEST_HANDOFF_PHASE_9_BUILD_27.md)

## What PolicyTrace does

PolicyTrace follows a source-first workflow:

1. Collect official Federal Register policy text and metadata.
2. Optionally load verified Regulations.gov public comments.
3. Discover related factual-reporting source pointers.
4. Build a deterministic source package before AI analysis begins.
5. Use Microsoft Foundry to generate structured policy findings.
6. Verify claims against the exact cited source passages.
7. Require human review before findings move into the final report.
8. Produce both a concise **Leadership Report** and a detailed **Evidence Audit Log**.

The reviewer can inspect the exact stored passage behind a finding and open the original official source.

Verification states include supported, partially supported, unsupported, and needs-human-review. A deterministic citation-integrity gate blocks claims that do not have a valid evidence trail.

## Why it is different

PolicyTrace does not treat an AI summary as the final product.

The workflow keeps separate:

- the AI-generated claim;
- the evidence cited for that claim;
- semantic verification of whether the evidence supports it;
- the model's confidence;
- the human review decision; and
- the final report-promotion state.

A finding can be withheld from the Leadership Report and still remain visible in the Evidence Audit Log. Nothing disappears simply because the AI was wrong.

## Live contest validation

The contest build was validated end-to-end with a real Microsoft Foundry deployment using `gpt-5-mini`.

The validated demo path includes:

- Federal Register intake
- current-status / freshness checks
- Regulations.gov comment sampling
- reproducible random sampling
- Microsoft Foundry analysis
- claim verification
- supported and partially-supported findings
- no-evidence fail-closed behavior
- human flagging and review
- media-source curation
- Leadership Report generation
- Evidence Audit Log generation
- explicit final human approval

The presentation includes the Microsoft Foundry monitor captured during testing.

## Demo policy

The rehearsed contest demo uses:

- Federal Register document: `2025-00636`
- Policy: **Framework for Artificial Intelligence Diffusion**
- RIN: `0694-AJ90`
- Regulations.gov docket: `BIS-2025-0001`
- Report standard: **Balanced**
- Microsoft Foundry model: `gpt-5-mini`

## Run the application

From the repository root:

```bash
python -m pip install -r backend/requirements.txt
python backend/run_app.py
```

Then open:

`http://127.0.0.1:8777/`

The current product UI is served from `frontend/app/`.

The older `frontend/demo/` and `backend/api.py` path are retained only as historical/reference material and are **not** the current contest application.

## Frontend architecture

PolicyTrace deliberately uses a lightweight browser frontend instead of a large JavaScript framework.

The current product UI lives in:

- `frontend/app/index.html` — page structure and controls
- `frontend/app/styles.css` — visual styling
- `frontend/app/app.js` — browser interactions and API calls

The frontend is served by the Python application entry point:

```bash
python backend/run_app.py
```

That server exposes the same tested backend review and analysis rules used by the application and serves `frontend/app/` at:

```text
http://127.0.0.1:8777/
```

This setup was intentional for the contest build:

- it keeps the demo portable and easy to run locally;
- judges can inspect the HTML, CSS, JavaScript, and Python directly in the repository;
- the browser UI talks to the backend over HTTP rather than duplicating verification or approval rules in client-side code;
- the backend remains the authority for review gates, refusals, evidence checks, and final approval behavior.

The frontend therefore focuses on presenting the workflow clearly — source intake, findings, evidence, reviewer actions, Leadership Report, and Evidence Audit Log — while the Python backend enforces the actual analysis and review logic.

## Microsoft Foundry

For the exact configuration used in the live validation, see:

**[MICROSOFT_FOUNDRY_SETUP.md](./MICROSOFT_FOUNDRY_SETUP.md)**

The important endpoint detail is that PolicyTrace expects the **Microsoft Foundry Models resource endpoint**, not a project-level `/api/projects/.../responses` URL.

Secrets are intentionally not stored in this repository or in saved project JSON.

## Development history and original working repository

This repository is the clean judge-facing mirror.

The full development history is preserved in the original repository:

- **Original development repository:** https://github.com/steveonw/microsoft-letastlator-thing
- **Contest working branch:** https://github.com/steveonw/microsoft-letastlator-thing/tree/fix/contest-phase1-comment-resilience
- **Latest AI handoff:** https://github.com/steveonw/microsoft-letastlator-thing/blob/fix/contest-phase1-comment-resilience/docs/AI_HANDOFF_CURRENT.md

The mirror also keeps the phase-by-phase engineering handoffs in `docs/`, including the path from the early analysis workflow through build 27.

## Repository guide

- `frontend/app/` — current PolicyTrace web interface
- `backend/run_app.py` — local application server
- `backend/foundry_client.py` — Microsoft Foundry Chat Completions client
- `backend/federal_register.py` — Federal Register intake/search/normalization
- `backend/policy_intake.py` — source-package intake workflow
- `docs/` — engineering handoffs, readiness notes, and collaboration documentation
- `TEST.md` — test guidance and validation notes

## Product principle

> **You do not have to trust the AI — here is the evidence trail.**
