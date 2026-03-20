# Changelog

All notable changes to VaultRAG are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).  
Versioning follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

Items designed and documented but not yet implemented. Full specifications in [page-06.html](https://raosiddharthp.github.io/VaultRAG/page-06.html).

### Planned for v1.0
- JWT authentication and role-based access control
- Department-level document namespacing in ChromaDB
- Immutable append-only audit log (ISO 9001 ready)
- LlamaGuard integration on G4 trigger (conditional, not always-on)
- Whisper local STT for fully air-gapped facilities
- Word (.docx) and PowerPoint (.pptx) document ingestion
- Text-to-speech response readback via Web Speech Synthesis API
- Document version management and auto re-index triggers
- Giskard automated vulnerability scanning in CI/CD pipeline

---

## [0.1.0] — 2026-03-20

Initial release. MVP portfolio artifact demonstrating the core RAG loop with 5-layer guardrail pipeline.

### Added

**Core pipeline**
- 5-layer guardrail pipeline: G1 Query Normaliser, G2 Scope Guard, G3 Confidence Threshold, G4 Safety Flag, G5 Citation Enforcer
- Voice input via Web Speech API — browser-native, zero install, zero server overhead
- Text fallback input for high-noise environments
- Llama 3.2 3B local inference via Ollama — no external API calls
- nomic-embed-text embeddings via Ollama — single process, no separate embedding service
- ChromaDB vector store — embedded, persistent to local disk
- LlamaIndex RAG orchestration — ingestion pipeline, query engine, response synthesis
- Procedural section chunking — splits at heading/procedure boundaries, not token windows
- Top-k=3 retrieval with cosine similarity scoring
- Configurable confidence threshold (default: 0.70)
- Mandatory safety prefix on G4 trigger (LOTO, high-voltage, pressure vessel keywords)
- Citation enforcement with one-retry logic on G5 failure

**Frontend & backend**
- FastAPI backend serving API and static files
- Single mobile-responsive HTML/CSS/JS frontend — no JavaScript framework
- One-button voice interface designed for one-handed factory floor use
- Step-by-step response format (max 5 steps) enforced by system prompt
- Source citation block on every successful response

**Document ingestion**
- PDF ingestion via PyMuPDF with section-boundary chunking
- TXT ingestion with heading-aware chunking
- Chunk metadata tagging: document title, section number, page range
- ChromaDB index persistence between sessions

**Deployment**
- Docker + Docker Compose single-command deployment
- Works identically on local machine, HuggingFace Spaces, and on-prem server
- No internet connection required after initial model download

**Portfolio documentation**
- `index.html` — Landing page: hook, forcing functions, architecture teaser, metrics
- `page-02.html` — The Problem: business narrative, human narrative, economic model with cited sources
- `page-03.html` — Architecture & Design: anchor client (FlexForm), 3 design principles, 8 ADRs, system diagram, deployment model, chunking strategy comparison
- `page-04.html` — Workflow Simulator: 4 pre-scripted scenarios with live pipeline animation
- `page-05.html` — Cost Model: problem cost vs solution cost, component breakdown, ROI calculation, competitive comparison
- `page-06.html` — Roadmap: MVP scope table, v1.0 path, deferred decisions, portfolio signal by discipline
- `style.css` — Shared design system: tokens, nav, footer, utilities, animations
- `README.md` — Project overview, setup instructions, stack reference
- `GLOSSARY.md` — Technical and domain term definitions
- `CHANGELOG.md` — This file

### Architecture Decisions

Eight Architecture Decision Records documented in page-03.html:

| ADR | Decision | Supersedes |
|---|---|---|
| ADR-001 | Llama 3.2 3B over Llama 3.1 8B | Original notes specified 3.1 8B |
| ADR-002 | nomic-embed-text over Sentence-BERT | Original notes listed both redundantly |
| ADR-003 | Custom prompt guardrails over LlamaGuard + Giskard | Original notes specified both as runtime components |
| ADR-004 | FastAPI + HTML over Streamlit | Original notes specified Streamlit |
| ADR-005 | Web Speech API over Whisper | Voice not specified in original notes |
| ADR-006 | HuggingFace Spaces over GCP / Render | GCP credits reserved for other projects; Render cold-start risk |
| ADR-007 | ChromaDB over Pinecone / Weaviate | Cloud vector stores violate data sovereignty invariant |
| ADR-008 | Procedural chunking over token windows | Default token windows split procedures unsafely |

### Known Limitations

- **Web Speech API STT** uses Google or Apple backend — violates strict air-gapped requirements. Mitigated in v1.0 with local Whisper.
- **Single document corpus** — no namespace isolation between departments. Mitigated in v1.0 with ChromaDB collections per namespace.
- **No user authentication** — system is open to anyone on the plant network. Mitigated in v1.0 with JWT auth.
- **No audit log** — queries and responses are not persisted for review. Mitigated in v1.0 with append-only log.
- **Demo environment** (HuggingFace Spaces) is not air-gapped — data sovereignty only applies in on-prem production deployment. Documented in portfolio.
- **Llama 3.2 3B** has less reasoning depth on complex multi-step procedures than 8B. Demo constraint only — production path uses 8B+.
- **G4 Safety Flag** uses keyword matching — susceptible to novel hazard phrasing not in the keyword list. Mitigated in v1.0 with conditional LlamaGuard.

### Dependencies

| Package | Version | Licence |
|---|---|---|
| llama-index | ≥0.10 | MIT |
| chromadb | ≥0.4 | Apache 2.0 |
| fastapi | ≥0.110 | MIT |
| uvicorn | ≥0.29 | BSD |
| pymupdf | ≥1.24 | AGPL / commercial |
| ollama | ≥0.1 | MIT |
| python-multipart | ≥0.0.9 | Apache 2.0 |

Model licences: Llama 3.2 (Meta Llama 3.2 Community Licence), nomic-embed-text (Apache 2.0).

---

## [0.0.1] — 2026-01-15 — Pre-release design notes

### Added
- Initial concept documentation (unrefined): index.html, TECH_STACK.html, GUARDRAIL_RULES.html, SAMPLE_QUERIES.html
- Stack proposal: LlamaIndex + Chroma + Ollama + Streamlit + LlamaGuard + Giskard + Sentence-BERT

### Notes
These files represent early design thinking prior to the architecture refinement process documented in the portfolio. They are retained in git history for reference. All component decisions were subsequently reviewed and several were revised — see ADR-001 through ADR-008 for the full decision trail.

---

[Unreleased]: https://github.com/raosiddharthp/VaultRAG/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/raosiddharthp/VaultRAG/releases/tag/v0.1.0
[0.0.1]: https://github.com/raosiddharthp/VaultRAG/releases/tag/v0.0.1
