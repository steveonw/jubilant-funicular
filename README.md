# PolicyTrace

> **Traceable AI for policy and public sentiment analysis**

**Team:** Steveon Walker & Dreshawn Young  
**Contest build:** Build 27  
**AI provider used in live validation:** Microsoft Foundry  
**Model:** `gpt-5-mini`

PolicyTrace is an evidence-grounded policy analysis application built to help analysts move faster without asking them to blindly trust AI.

It combines official policy text, public comments, and related reporting into a structured review workflow. Important AI-generated findings are tied back to evidence, checked for support, and kept under human review before they reach the final leadership brief.

> **You do not have to trust the AI — here is the evidence trail.**

---

## For judges: start here

PolicyTrace is designed around one simple rule:

> **No important AI claim without evidence. No evidence without a support check. No final report without human review.**

A typical PolicyTrace run follows this path:

```text
Official policy sources
        |
        v
Source package
        |
        v
Microsoft Foundry analysis
        |
        v
Claim + evidence verification
        |
        v
Human review
        |
        +-------------------+
        |                   |
        v                   v
Leadership Report     Evidence Audit Log
```

The **Leadership Report** is the concise decision-facing output.

The **Evidence Audit Log** is the receipt layer: it preserves reviewed claims, verification state, reviewer action, source metadata, and exact stored evidence passages.

---

## Team

**Steveon Walker**  
**Dreshawn Young**

PolicyTrace was built for the Microsoft + CCI Innovation Challenge as an evidence-first approach to policy and public sentiment analysis.

---

## What PolicyTrace does

PolicyTrace can:

- search and load Federal Register policy documents;
- build a source package before AI analysis begins;
- detect and use confirmed Regulations.gov dockets conservatively;
- retrieve and reproducibly sample public comments;
- discover related factual-reporting source pointers;
- run policy analysis through Microsoft Foundry;
- attach stable claim IDs and exact evidence passages;
- distinguish supported, partially supported, unsupported, and human-review-needed findings;
- block claims that fail citation-integrity checks;
- allow a reviewer to edit, flag, verify, and approve findings;
- generate a concise **Leadership Report**;
- preserve the full review history in an **Evidence Audit Log**;
- require an explicit final human approval step.

The application intentionally keeps AI interpretation, source evidence, verification status, model confidence, and human review state separate.

---

## Why PolicyTrace is different

PolicyTrace does not treat a generated summary as the finished product.

For an important finding, the reviewer can inspect:

1. **What the AI claimed**
2. **What source evidence was cited**
3. **Whether that evidence actually supports the claim**
4. **What the human reviewer decided**
5. **Whether the claim is allowed into the final report**

A finding can be withheld from the Leadership Report and still remain preserved in the Evidence Audit Log.

Nothing disappears simply because the AI was wrong.

---

## Live contest validation

The contest build was validated end-to-end using a real Microsoft Foundry deployment with `gpt-5-mini`.

The validated workflow included:

- Federal Register intake
- source-package construction before AI analysis
- current-status / freshness checks
- Regulations.gov comment ingestion
- seeded random comment sampling
- live Microsoft Foundry analysis
- claim verification
- supported and partially-supported findings
- fail-closed behavior for claims with no cited evidence
- human reviewer flags and section review
- related-media selection / exclusion
- Leadership Report generation
- Evidence Audit Log generation
- explicit final human approval

Microsoft Foundry Monitor usage was captured during development and rehearsal as evidence that the deployed model processed the PolicyTrace workload.

---

## Demo policy

The primary contest rehearsal used:

| Item | Value |
|---|---|
| Federal Register document | `2025-00636` |
| Policy | **Framework for Artificial Intelligence Diffusion** |
| RIN | `0694-AJ90` |
| Regulations.gov docket | `BIS-2025-0001` |
| Report standard | **Balanced** |
| Foundry model | `gpt-5-mini` |

The rehearsal also demonstrated that PolicyTrace carries corpus limitations forward rather than presenting a small comment sample as representative of the general public.

---

## Run PolicyTrace locally

Clone this judge-facing mirror:

```bash
git clone https://github.com/steveonw/jubilant-funicular.git
cd jubilant-funicular
```

Install the backend requirements and start the app:

```bash
python -m pip install -r backend/requirements.txt
python backend/run_app.py
```

Then open:

```text
http://127.0.0.1:8777/
```

The current contest UI is served from `frontend/app/`.

---

## Frontend architecture

PolicyTrace deliberately uses a lightweight HTML/CSS/JavaScript frontend so the contest build is easy to inspect and easy to run.

The product UI lives in:

- `frontend/app/index.html` — page structure and controls
- `frontend/app/styles.css` — styling
- `frontend/app/app.js` — browser interactions and API requests

The frontend is served by:

- `backend/run_app.py` — local application entry point

The browser talks to the Python backend over HTTP. Important verification and approval rules are **not duplicated in client-side JavaScript**.

The backend remains responsible for:

- evidence integrity checks;
- semantic verification;
- review gates and refusals;
- report-promotion rules;
- final approval behavior.

That separation lets the frontend focus on making the workflow understandable while the backend remains the source of truth for the review process.

---

## Microsoft Foundry setup

The live contest validation used:

- **Provider:** Microsoft Foundry
- **Model / deployment:** `gpt-5-mini`
- **Resource endpoint:** `https://<your-resource>.services.ai.azure.com`
- **Authentication used in rehearsal:** API key

> The `policytrance` endpoint spelling is intentional.

PolicyTrace expects the **Microsoft Foundry Models resource endpoint**, not a project-level `/api/projects/.../responses` URL.

Full setup instructions:

**[MICROSOFT_FOUNDRY_SETUP.md](./MICROSOFT_FOUNDRY_SETUP.md)**

The setup guide documents both the in-app configuration and Git Bash environment-variable configuration.

**No real API key or bearer token is stored in this repository.**

---

## Repository guide

| Path | Purpose |
|---|---|
| `frontend/app/` | Current PolicyTrace web interface |
| `backend/run_app.py` | Application server entry point |
| `backend/foundry_client.py` | Microsoft Foundry client |
| `backend/federal_register.py` | Federal Register intake/search/normalization |
| `backend/policy_intake.py` | Source-package intake workflow |
| `docs/` | Engineering handoffs and contest readiness documentation |
| `TEST.md` | Test and validation guidance |

The older `frontend/demo/` / legacy demo path is retained only as historical reference and is **not** the current contest product.

---

## AI-assisted development disclosure

PolicyTrace was developed with **AI-assisted coding and multi-model review** as part of the implementation process.

**ChatGPT was the primary AI coding collaborator** used throughout the project to help plan, draft, review, debug, document, and refine the application. **Claude Opus** was also used extensively for debugging and second-pass technical review.

During development, the team also used **OpenRouter** to compare feedback from multiple major model families, including models from **OpenAI, Anthropic (Claude), Google, and xAI (Grok)**. We informally treated this as a small “AI council”: different models were asked to inspect problems, challenge assumptions, or suggest debugging directions, while the team decided what to accept, test, or reject.

These development assistants are separate from the application’s runtime AI path. The contest build itself was validated using **Microsoft Foundry with `gpt-5-mini`**.

AI assistance did not replace human ownership of the project. The team directed the product design, evidence rules, review workflow, integrations, testing, and final contest submission. Generated suggestions were treated as inputs to be checked rather than automatically trusted or merged.

The live application, Microsoft Foundry integration, evidence-verification workflow, human-review gates, and final outputs were tested as part of the project rather than presented as unverified generated code.

## Development history

This repository is the clean, judge-facing mirror.

The original repository preserves the longer development trail and working branch:

- **Original development repository:** https://github.com/steveonw/microsoft-letastlator-thing
- **Contest working branch:** https://github.com/steveonw/microsoft-letastlator-thing/tree/fix/contest-phase1-comment-resilience
- **Latest AI handoff:** https://github.com/steveonw/microsoft-letastlator-thing/blob/fix/contest-phase1-comment-resilience/docs/AI_HANDOFF_CURRENT.md

The mirror also includes the phase-by-phase engineering handoffs under `docs/`, showing how the project progressed into Build 27.

---

## Contest documentation

- [Microsoft Foundry setup](./MICROSOFT_FOUNDRY_SETUP.md)
- [Current contest handoff](./docs/CONTEST_HANDOFF_CURRENT.md)
- [Build 27 handoff](./docs/CONTEST_HANDOFF_PHASE_9_BUILD_27.md)
- [Contest readiness plan](./docs/CONTEST_READINESS_PLAN.md)

The PowerPoint and video are part of the contest submission package. If the PowerPoint is also added to this repository, it should be placed in the repository root as `PolicyTrace_Contest_Presentation.pptx`.

---

## Product principle

> **You do not have to trust the AI - here is the evidence trail..**
