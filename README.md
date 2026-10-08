# 🎓 Coursera Multimodal Intelligence Platform

[![Production Status](https://img.shields.io/badge/Production-Live%20%26%20Certified-success?style=for-the-badge&logo=vercel)](https://coursera-multimodal-intelligence-pl.vercel.app/)
[![Automated Tests](https://img.shields.io/badge/Tests-97%2F97%20Passed-brightgreen?style=for-the-badge&logo=pytest)](https://github.com/abhik99/Coursera-Multimodal-Intelligence-Platform)
[![Python Version](https://img.shields.io/badge/Python-3.11%20%7C%203.12%20%7C%203.13-blue?style=for-the-badge&logo=python)](https://www.python.org/)
[![React Version](https://img.shields.io/badge/Frontend-React%2019%20%2B%20Vite-61DAFB?style=for-the-badge&logo=react)](https://react.dev/)
[![FastAPI](https://img.shields.io/badge/Backend-FastAPI-009688?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![pgvector](https://img.shields.io/badge/Vector%20DB-PostgreSQL%2016%20%2B%20pgvector-336791?style=for-the-badge&logo=postgresql)](https://github.com/pgvector/pgvector)
[![Gemini LLM](https://img.shields.io/badge/LLM-Google%20Gemini%202.5%20Flash-8E75B2?style=for-the-badge&logo=google)](https://ai.google.dev/)

An enterprise-grade, evidence-grounded multimodal learning analytics and conversational AI platform designed to transform raw Coursera course archives into clean, structured, validated, and RAG-ready knowledge graphs.

The platform ingests video transcripts (SRT/TXT), HTML readings, assignments, and curriculum hierarchies, index them with 384-dimensional dense vector embeddings in `pgvector`, and powers real-time instructional diagnostics alongside an anti-hallucination conversational AI teaching assistant.

---

## 🌐 Live Production Deployment
* **Video Presentation:** [Watch the Project Video](https://drive.google.com/file/d/1GIi0RfJnWrjhge4tGiIoH9kWM9z6d-rx/view?usp=sharing)
* **Live Web Application:** [https://coursera-multimodal-intelligence-pl.vercel.app/](https://coursera-multimodal-intelligence-pl.vercel.app/)
* **Backend API Documentation (Swagger):** `/docs` on active backend host
* **Hosting Infrastructure:** Vercel Global Edge CDN + Supabase Cloud PostgreSQL + Render API Web Service

![Coursera Multimodal Intelligence Platform Live Dashboard](docs/images/dashboard_overview.png)

---

## 👥 Team & Engineering Contributions

| Engineering Tier | Lead Engineers | Focus Area & Deliverables |
| :--- | :--- | :--- |
| **Database & Preprocessing** | **Chetan Kailas Patil** ([GitHub](https://github.com/Chetanpatil71502/))<br>**Sona Christina A T** ([Email](mailto:sonachristina15@gmail.com)) | PostgreSQL schema, SRT subtitle parsers, HTML reading sanitizer, sentence chunker, SHA-256 asset registry, and data quality validation. |
| **Backend (API & Orchestration)** | **Tushar** ([Email](mailto:tushar.10012003@gmail.com))<br>**Gaurav Jagannath Kadam** ([GitHub](https://github.com/Gaurav0358)) | FastAPI microservices, course analysis routers, course ingestion lifecycle, SQL models, processing job monitors, and REST schemas. |
| **AI / RAG & LLM Engine** | **Sakshi Kumari** ([GitHub](https://github.com/SakshiBhardwaj27))<br>**Megha Mahesh Kanavi** ([Email](mailto:meghakanavi.uk@gmail.com)) | `all-MiniLM-L6-v2` dense embeddings, `pgvector` HNSW cosine similarity search, Gemini prompt engineering, and `EvidenceValidator` anti-hallucination guardrail. |
| **Frontend Engineering** | **P Sankarshan** ([GitHub](https://github.com/sankarshan07)) | React 19 SPA, Tailwind CSS, telemetry analytics dashboard, interactive diagnostic modals, course management, and conversational chat UI. |
| **Testing, Integration & Deployment** | **Abhishek Kumar** ([GitHub](https://github.com/abhik99)) | Automated pytest test suites, Vite production optimization, end-to-end integration, Supabase migration scripts, Vercel edge deployment, and audit reports. |

---

## 📚 Validation Dataset

* **Course:** **IBM Data Science Professional Certificate** (Used for end-to-end benchmarking, validation, and testing)
* **Archive Format:** Authorized local course archive (`.zip`) containing multimodal course assets
* **Immutability:** The raw archive acts as an immutable source of truth; data ingestion reads and cleanses assets without modifying original files.
* **Generalizability:** While validated against the IBM Data Science curriculum, the pipeline and ingestion engines are architected to ingest **any authorized course archive**.

---

## 🏗️ End-to-End System Architecture

```mermaid
graph TD
    subgraph Data Ingestion & Preprocessing
        A["Raw Course Archive (.zip)"] --> B["Multimodal Ingestion Pipeline"]
        B --> C1["SRT Subtitle Parser (100ms Millisecond Sync)"]
        B --> C2["HTML Reading Cleaner (<co-content> Sanitizer)"]
        B --> C3["Semantic Sentence Chunker (~387 avg tokens)"]
        B --> C4["Deterministic SHA-256 Checksum Registry"]
    end

    subgraph Storage & Vector Indexing
        C1 & C2 & C3 & C4 --> D[("PostgreSQL 16 Relational Schema (12 Tables)")]
        D --> E["Embedding Engine (all-MiniLM-L6-v2, 384-dim)"]
        E --> F[("pgvector HNSW Cosine Index (9,479 Embeddings)")]
    end

    subgraph AI / RAG & Anti-Hallucination Guardrails
        G["User Question"] --> H["Retriever Engine (Cosine Distance <= 0.40)"]
        F --> H
        H --> I["Top-K Course Evidence Chunks"]
        I --> J["Grounded Prompt Synthesizer"]
        J --> K["Google Gemini 2.5 Flash LLM"]
        K --> L["EvidenceValidator (Rejects Unverified Citation IDs)"]
        L --> M["Grounded Answer + Multimodal Evidence Badges"]
    end

    subgraph API & Edge Client Presentation
        D & M --> N["FastAPI Backend Router (/courses, /analysis, /chat)"]
        N --> O["React 19 + Tailwind CSS Frontend Client"]
        O --> P["Vercel Global Edge Network (289ms TTFB)"]
    end
```

### High-Speed Conversational RAG Sequence

```mermaid
sequenceDiagram
    autonumber
    actor Learner as Learner / Instructor
    participant Frontend as React 19 Frontend (Vercel Edge)
    participant Backend as FastAPI Backend (Render / Docker)
    participant VectorDB as Supabase (PostgreSQL 16 + pgvector)
    participant LLM as Google Gemini 2.5 Flash
    participant Guardrail as EvidenceValidator

    Learner->>Frontend: Submit question (e.g., "Explain Big Data vs Data Mining")
    Frontend->>Backend: POST /chat/ { question, course_title }
    Backend->>Backend: Generate 384-dim query vector (sentence-transformers)
    Backend->>VectorDB: Query cosine similarity (<=> operator, threshold >= 0.40)
    VectorDB-->>Backend: Return Top-5 grounded transcript & reading chunks
    Backend->>LLM: Send structured RAG prompt with verbatim course context
    LLM-->>Backend: Return synthesized explanation + citation IDs [E1, E2]
    Backend->>Guardrail: Cross-reference cited IDs against retrieved evidence
    Guardrail-->>Backend: Verified (No hallucinated citation IDs detected)
    Backend-->>Frontend: JSON payload { answer, evidence, confidence: 0.95 }
    Frontend-->>Learner: Render formatted Markdown response with clickable citations
```

---

## 📊 Comprehensive Implementation Status

| Component | Status | Metrics & Implementation Details |
| :--- | :---: | :--- |
| **Data Ingestion** | ✅ Complete | 157 source files parsed without error; raw archive preserved intact. |
| **SRT Processing** | ✅ Complete | 45 SRT files; 3,280 subtitle segments; 0 empty captions; 0 invalid timestamps. |
| **TXT Chunking** | ✅ Complete | 45 TXT files; 102 semantic chunks; ~387 avg tokens; 100% timestamp-aligned. |
| **HTML Sanitization** | ✅ Complete | 22 HTML readings; 19 RAG-enabled; 3 administrative files cleanly categorized. |
| **Data Quality Engine** | ✅ Complete | 0 orphan records; 0 duplicate asset slugs; 0 SHA-256 collisions. |
| **Relational Database** | ✅ Complete | 12 relational PostgreSQL tables maintaining complete pedagogical hierarchy. |
| **Vector Database** | ✅ Complete | `pgvector` HNSW index populated with 9,479 dense 384-dimensional embeddings. |
| **AI / RAG Pipeline** | ✅ Complete | Grounded retrieval with 0.40 cosine threshold and out-of-scope safety guardrail. |
| **Anti-Hallucination** | ✅ Complete | `EvidenceValidator` strictly rejects fabricated evidence keys and IDs. |
| **Backend REST API** | ✅ Complete | FastAPI endpoints (`/courses`, `/analysis`, `/chat`) with interactive Swagger docs. |
| **Frontend Application** | ✅ Complete | React 19 SPA with telemetry cards, interactive modals, and conversational chat. |
| **Automated Testing** | ✅ Complete | **97/97 tests passed** across multimodal parsers, AI guardrails, and builds. |
| **Cloud Deployment** | ✅ Complete | Live on Vercel Global Edge Network with free-tier Supabase & Render integration. |

---

## 🛠️ Technology Stack

```text
┌───────────────────────────────────────────────────────────────────────────┐
│                           TECHNOLOGY STACK                                │
├──────────────────────────┬────────────────────────────────────────────────┤
│ Layer                    │ Technologies                                   │
├──────────────────────────┼────────────────────────────────────────────────┤
│ Frontend SPA             │ React 19, Vite, Tailwind CSS, Emotion,         │
│                          │ Material UI Icons, React Markdown, Axios       │
│ Backend API              │ FastAPI, Uvicorn, SQLAlchemy 2.0, Pydantic v2, │
│                          │ Python 3.11 / 3.12 / 3.13                      │
│ AI / RAG Engine          │ Google Gemini 2.5 Flash, LangChain,            │
│                          │ sentence-transformers (all-MiniLM-L6-v2)       │
│ Vector Database          │ PostgreSQL 16 + pgvector (HNSW Index, 384-dim) │
│ Ingestion & Processing   │ BeautifulSoup4, FFmpeg, ffprobe, psycopg3      │
│ Testing & Quality        │ pytest, pytest-asyncio, Vite build validator   │
│ DevOps & Deployment      │ Docker, Docker Compose, Vercel Edge, Render,   │
│                          │ Supabase Cloud, GitHub Actions CI              │
└──────────────────────────┴────────────────────────────────────────────────┘
```

---

## 📈 Benchmark Dataset Details

The **IBM Data Science Professional Certificate** validation benchmark dataset details:

| Metric | Measured Value | Validation Note |
| :--- | :---: | :--- |
| **Total Source Files** | **157** | Complete multimodal course package |
| **Video Files** | **45** | Registered in asset catalog with metadata |
| **SRT Transcripts** | **45** | Millisecond-level caption synchronization |
| **TXT Transcripts** | **45** | Cleaned and partitioned into semantic chunks |
| **HTML Readings** | **22** | Stripped of boilerplate with `<co-content>` extraction |
| **Course Modules** | **4** | Preserves sequential hierarchy and optional flags |
| **Lesson Groups** | **12** | Pedagogical groupings |
| **Lesson Records** | **65** | Individual learning activities |
| **RAG-Eligible Assets** | **154** | Administrative assets (3) excluded from embeddings |
| **Vector Embeddings** | **9,479** | Dense 384-dimensional vectors stored in `pgvector` |

### Course Curriculum Hierarchy

```text
IBM Data Science Professional Certificate
│
├── Module 01: Defining Data Science and What Data Scientists Do
│   ├── Welcome To The Course
│   ├── Defining Data Science
│   └── What Do Data Scientists Do
│
├── Module 02: Data Science Topics
│   ├── Big Data And Data Mining
│   └── Deep Learning And Machine Learning
│
├── Module 03: Applications and Careers in Data Science
│   ├── Data Science Application Domains
│   ├── Careers And Recruiting
│   ├── Final Assignment
│   ├── Course Wrap Up
│   └── Digital Badge
│
└── Module 04: Data Literacy for Data Science (Optional — flagged is_optional = true)
    ├── Understanding Data
    └── Data Literacy
```

---

## 🗄️ Database Architecture & Schema Design

The PostgreSQL database maintains strict referential integrity across 12 relational tables:

```text
courses (Root course metadata, URL, provider, status)
  │
  ├── course_modules (Modules, sequence numbers, is_optional flag)
  │     │
  │     └── lesson_groups (Curricular lesson groupings)
  │            │
  │            └── lessons (Individual lectures, readings, quizzes)
  │                   │
  │                   └── assets (Master registry, SHA-256 checksum, MIME, RAG flag)
  │                          ├── videos (Duration, resolution, audio specs)
  │                          ├── video_segments (Video timecodes)
  │                          ├── transcripts (Format: SRT/TXT, language)
  │                          ├── transcript_segments (Timecodes, text, embeddings)
  │                          └── readings (HTML reading text, category, embeddings)
  │
  ├── processing_jobs (Pipeline execution logs, stage timers, errors)
  └── data_quality_issues (Automated audit logs, severity, resolution)
```

### Asset Traceability & Source Lineage

Every generated embedding and AI citation maintains a direct lineage trail back to its source:

$$\text{Course} \longrightarrow \text{Module} \longrightarrow \text{Lesson} \longrightarrow \text{Asset} \longrightarrow \text{Source File} \longrightarrow \text{Segment / Timecode}$$

---

## 🤖 AI / RAG Engine & Guardrail Architecture

The AI layer bridges course knowledge with generative AI using a defense-in-depth architecture:

1. **Dense Vector Embeddings:**
   All RAG-enabled transcript segments and HTML readings are embedded using `sentence-transformers/all-MiniLM-L6-v2` into 384-dimensional vector spaces.
2. **HNSW Indexed Vector Search:**
   Vector retrieval is executed directly inside PostgreSQL via `pgvector` using cosine distance (`<=>` operator):
   ```sql
   SELECT id, content, citation_id, 1 - (embedding <=> query_vector) AS similarity
   FROM transcript_segments
   WHERE rag_enabled = true AND 1 - (embedding <=> query_vector) >= 0.40
   ORDER BY similarity DESC
   LIMIT 5;
   ```
3. **Relevance Thresholding & Safety Guards:**
   If the maximum retrieved cosine similarity falls below `0.40`, or if the query falls outside the course curriculum, the model declines to answer rather than speculating:
   > *"The available course evidence is insufficient to answer this question."*
4. **Anti-Hallucination Citation Guardrail (`EvidenceValidator`):**
   The output from Google Gemini 2.5 Flash is strictly validated. If the generated response references a citation ID (e.g., `fake-evidence-999`) that was not present in the retrieved evidence set, the response is rejected and sanitized.
5. **Direct Client Fallback Synthesis:**
   To guarantee rapid response times (under 2 seconds) even during backend cold starts, the frontend includes an asynchronous fallback route connecting directly to Gemini 2.5 Flash with cached course syllabus context.

---

## 🖥️ Interactive Frontend Features & UI Showcase

The React 19 single-page application offers an intuitive, real-time diagnostic interface for students, instructors, and curriculum designers:

### 1. Instructional Telemetry Dashboard
Real-time counters for Analyzed Courses, Detected Issues, Pedagogical Recommendations, and Pending Reviews with micro-animations and navigation affordances (`Inspect →`, `Explore →`, `Review →`).

![Production Dashboard Overview](docs/images/dashboard_overview.png)

---

### 2. ⚠️ Detected Issues & Diagnostic Telemetry Modal
Inspect friction points categorized by *Pacing & Replay*, *Concept Confusion*, and *Quiz & Labs*. Displays telemetry indicators (e.g., `81% Replay Density Spike • 76% Row Duplication`) and concrete remediation plans with direct AI Chat routing.

![Detected Issues Modal](docs/images/issues_modal.png)

---

### 3. ✅ Pedagogical Recommendations & Optimizations Modal
Actionable pedagogical enhancements (*Interactive Checkpoints*, *Explanatory Analogies*, *Code & Benchmarks*) paired with projected impact metrics (e.g., `🚀 -65% Duplicate Join Errors`, `+55% SQL Quiz Accuracy`) and verified multimodal evidence citations.

![Pedagogical Recommendations Modal](docs/images/recommendations_modal.png)

---

### 4. 🕒 Pending Course Reviews & QA Verification Modal
4-step Multimodal Ingestion & Verification Checklist (*Transcript Sync*, *Reading Sanitization*, *Vector Embeddings*, *Instructor Sign-off*) with 1-click verification (`✓ Approve & Mark Verified`).

![Pending Course Reviews Modal](docs/images/pending_reviews_modal.png)

---

### 5. 🤖 Conversational AI Teaching Assistant & Grounded Citations
Real-time conversational assistant providing structured Markdown responses, SQL code syntax highlighting, pedagogical analogies, and verified evidence citation badges (`[E1]`, `[E2]`). Prompt chips enable 1-click inquiries such as *"Suggest a better explanation"* or *"What are common misconceptions?"*.

![Conversational AI Assistant](docs/images/chat_assistant_response.png)

---

### 6. 📚 Course Ingestion & Management
Register courses by URL or archive, inspect materials and transcripts, monitor real-time ingestion status and processing steps, and manage course entries.

![Course Management](docs/images/course_management.png)

---

## 📁 Repository Directory Structure

```text
Coursera-Multimodal-Intelligence-Platform/
│
├── .env.example                                  # Template for backend environment variables
├── .gitignore                                    # Git exclusion rules (creds, dumps, caches)
├── docker-compose.yml                            # Multi-container orchestration (db, backend, frontend)
├── Dockerfile.backend                            # Production container image for FastAPI backend
├── render.yaml                                   # Infrastructure-as-code for Render deployment
├── DEPLOYMENT.md                                 # Complete production cloud deployment guide
├── integration_and_deployment_report.md          # Comprehensive integration & audit report
├── pytest.ini                                    # Configuration for automated test discovery
├── requirements.txt                              # Python root dependencies
├── main.py                                       # Root entrypoint exposing FastAPI backend app
│
├── AI_RAG/                                       # Core RAG & LLM Engine
│   ├── pipeline.py                               # Master RAGPipeline coordinator
│   ├── HANDOFF.md                                # Person 5 RAG architecture handoff doc
│   ├── README.md                                 # RAG module documentation
│   ├── embedding/
│   │   ├── embedder.py                           # SentenceTransformers 384-dim embedding encoder
│   │   ├── data_loader.py                        # Postgres segment loader
│   │   └── store_embeddings.py                   # pgvector batch insertion utility
│   ├── retrieval/
│   │   └── retriever.py                          # Cosine similarity vector search
│   └── llm/
│       ├── model.py                              # Google Gemini client wrapper
│       ├── prompts.py                            # Grounded prompt templates
│       ├── synthesizer.py                        # JSON synthesis handler
│       └── evidence_validator.py                 # Anti-hallucination citation validator
│
├── coursera_insight_backend/                     # FastAPI Backend Microservice
│   ├── main.py                                   # FastAPI app, CORS middleware, route registration
│   ├── config.py                                 # App configuration and settings
│   ├── database.py                               # DB engine & get_db dependency provider
│   ├── models.py                                 # Backend ORM models (Course, ProcessingJob, DQIssue)
│   ├── schemas.py                                # Pydantic request/response schemas
│   └── routers/
│       ├── courses.py                            # /courses/ endpoints (list, analyze, status)
│       ├── analysis.py                           # /analysis/ endpoints (dashboard stats, findings)
│       └── chat.py                               # /chat/ conversational RAG endpoint
│
├── frontend/                                     # React 19 Frontend Application
│   ├── package.json                              # Node dependencies (React 19, Vite, Tailwind, MUI)
│   ├── vite.config.js                            # Vite configuration
│   ├── vercel.json                               # Vercel SPA routing rewrites
│   ├── Dockerfile                                # Frontend production Nginx container
│   ├── nginx.conf                                # Production Nginx reverse proxy configuration
│   ├── index.html                                # HTML shell with Poppins & Inter typography
│   └── src/
│       ├── main.jsx                              # React root mounting
│       └── App.jsx                               # Complete interactive application SPA
│
├── preprocessing/                                # Multimodal Ingestion & Preprocessing
│   ├── run_pipeline.py                           # Master idempotent ingestion pipeline
│   ├── extract_course.py                         # Archive extraction and hierarchy discovery
│   ├── load_to_db.py                             # Relational DB loader
│   ├── common/                                   # Shared logging, config, and DB utilities
│   ├── transcript/                               # SRT and TXT transcript parsers
│   ├── html_proc/                                # HTML sanitizer and co-content parser
│   ├── video/                                    # Video metadata extractor
│   ├── assignment/                               # Assignment asset processor
│   └── quality/                                  # Automated data quality checks
│
├── database/                                     # Database Management & Migrations
│   ├── init_db.py                                # DB schema initializer script
│   ├── validate_phase3.py                        # 12-table relational validation test suite
│   ├── generate_sample_rag.py                    # RAG verification record generator
│   └── migrations/                               # SQL migration scripts
│
├── scripts/                                      # Deployment, Migration & Reporting Tools
│   ├── migrate_to_supabase.py                    # 1-click database & vector migration to Supabase
│   ├── generate_architecture_pdf.py              # Architecture diagram PDF generator
│   ├── generate_pdf_report.py                    # Integration audit report PDF generator
│   ├── generate_presentation_pdf.py              # Slide deck PDF generator
│   └── generate_presentation_pptx.py             # Slide deck PowerPoint (.pptx) generator
│
├── tests/                                        # Terminal Test Suite (93 Unit Tests)
│   ├── conftest.py                               # Pytest fixtures (sample SRT, TXT, HTML)
│   ├── test_srt_parser.py                        # SRT millisecond timecode parsing tests (21)
│   ├── test_txt_chunker.py                       # Semantic sentence chunking tests (13)
│   ├── test_html_extractor.py                    # HTML reading extraction tests (23)
│   └── test_utils.py                             # SHA-256 hash & metadata utility tests (36)
│
├── Ai_Tests/                                     # AI & Guardrail Test Suite
│   ├── test_evidence_validator.py                # Citation cross-referencing tests
│   ├── test_invalid_evidence.py                  # Hallucinated citation ID rejection tests
│   ├── test_relevance_guardrail.py               # Out-of-scope query guardrail tests
│   └── test_threshold.py                         # Cosine similarity cutoff tests
│
└── docs/                                         # Engineering Documentation & Data Contracts
    ├── images/                                   # Platform UI Screenshots & Architecture Assets
    │   ├── dashboard_overview.png                # Live dashboard overview
    │   ├── issues_modal.png                      # Detected issues diagnostic modal
    │   ├── recommendations_modal.png             # Pedagogical recommendations modal
    │   ├── pending_reviews_modal.png             # Pending course reviews QA checklist
    │   ├── chat_assistant_response.png           # Grounded conversational AI response
    │   └── course_management.png                 # Course ingestion & inventory management
    ├── course_structure.md                       # Discovered course hierarchy
    ├── asset_relationships.md                    # Multimodal asset mapping
    ├── database_design.md                        # Relational schema reference
    ├── data_quality_report.md                    # Data quality validation findings
    ├── phase3_database_validation_report.md      # Database certification report
    ├── ai_rag_data_contract.md                   # RAG schema interface specification
    └── sample_rag_records.json                   # Verified sample RAG JSON objects
```

---

## 🚀 Quickstart & Setup Guide

### Prerequisites

* **Python:** 3.11, 3.12, or 3.13
* **Node.js:** v18+ and npm
* **Docker & Docker Compose:** Optional, recommended for quickstart
* **PostgreSQL:** v16 with `pgvector` extension
* **Google Gemini API Key:** Free key from [Google AI Studio](https://aistudio.google.com/)

---

### Option 1: Full-Stack Docker Compose (Fastest)

Launch the complete stack (PostgreSQL + pgvector, FastAPI Backend, and React Frontend) with a single command:

```bash
# 1. Clone the repository
git clone https://github.com/abhik99/Coursera-Multimodal-Intelligence-Platform.git
cd Coursera-Multimodal-Intelligence-Platform

# 2. Configure environment variables
cp .env.example .env
# Open .env and add your GEMINI_API_KEY

# 3. Start all services
docker-compose up --build
```

Access the services:
* **Frontend UI:** `http://localhost:3000`
* **FastAPI Backend:** `http://localhost:8000`
* **Interactive API Docs:** `http://localhost:8000/docs`
* **PostgreSQL + pgvector:** `localhost:5433`

---

### Option 2: Local Development Setup

#### 1. Configure Python Environment

```bash
# Create and activate virtual environment
python -m venv .venv

# On Windows:
.venv\Scripts\activate
# On Linux / macOS:
source .venv/bin/activate

# Install backend dependencies
pip install -r requirements.txt
```

#### 2. Configure Database & Environment Variables

Copy `.env.example` to `.env` and fill in your credentials:

```env
DB_HOST=localhost
DB_PORT=5432
DB_NAME=coursera_platform
DB_USER=postgres
DB_PASSWORD=your_password
GEMINI_API_KEY=AIzaSy...your_gemini_key
GEMINI_MODEL=gemini-2.5-flash
```

Initialize the database schema:

```bash
python database/init_db.py
```

#### 3. Run the Ingestion Pipeline (Optional to re-populate)

Process the course archive and populate relational tables:

```bash
python preprocessing/run_pipeline.py --skip-video
```

#### 4. Launch the FastAPI Backend

```bash
uvicorn main:app --host 127.0.0.1 --port 8000 --reload
```

Test that the backend is live at `http://127.0.0.1:8000/health`.

#### 5. Launch the React Frontend

Open a new terminal:

```bash
cd frontend
npm install

# Configure frontend environment (optional, defaults to localhost:8000)
cp .env.example .env

# Start the Vite development server
npm run dev
```

Open your browser at `http://localhost:5173`.

---

## ⚡ Environment Variables Reference

### Backend (`.env`)

| Variable | Description | Default |
| :--- | :--- | :--- |
| `DB_HOST` | PostgreSQL hostname | `localhost` |
| `DB_PORT` | PostgreSQL port | `5432` |
| `DB_NAME` | Database name | `coursera_platform` |
| `DB_USER` | Database username | `postgres` |
| `DB_PASSWORD` | Database password | — |
| `DATABASE_URL` | SQLAlchemy / asyncpg connection string | Auto-constructed from above |
| `GEMINI_API_KEY` | Google Gemini API Key | — *(Required for RAG)* |
| `GEMINI_MODEL` | Gemini model variant | `gemini-2.5-flash` |

### Frontend (`frontend/.env`)

| Variable | Description | Default |
| :--- | :--- | :--- |
| `VITE_API_BASE_URL` | Address of the running FastAPI backend | `http://localhost:8000` |
| `VITE_GEMINI_API_KEY` | Optional direct client Gemini API key | Configurable in UI |

---

## 🧪 Comprehensive Automated Test Suites

The platform includes an automated multi-tier testing pipeline covering data ingestion, multimodal parsers, vector retrieval, AI anti-hallucination guardrails, and production client compilation.

### Run All Backend Multimodal Parser Tests (93 Tests)

```bash
pytest -q
```

```text
============================= test session starts =============================
platform win32 -- Python 3.12.10, pytest-9.1.1, pluggy-1.6.0
rootdir: C:\Coursera-Multimodal-Intelligence-Platform-main
configfile: pytest.ini
collected 93 items

tests\test_html_extractor.py .......................                     [ 24%]
tests\test_srt_parser.py .....................                           [ 47%]
tests\test_txt_chunker.py .............                                  [ 61%]
tests\test_utils.py ....................................                 [100%]

============================= 93 passed in 0.88s ==============================
```

### Run AI Evidence Guardrail & Anti-Hallucination Tests

```bash
pytest Ai_Tests/test_evidence_validator.py -q
python Ai_Tests/test_invalid_evidence.py
```

```text
===== INVALID EVIDENCE TEST =====
Validation correctly rejected the response.
Error: LLM referenced invalid evidence IDs: ['fake-evidence-999']
```

### Run Frontend Production Build Validation

```bash
cd frontend
npm run build
```

```text
vite v8.3.1 building client environment for production...
✓ 16 modules transformed.
dist/index.html                  1.30 kB │ gzip:   0.61 kB
dist/assets/index-CtmCS7qS.js  391.07 kB │ gzip: 107.97 kB
✓ built in 270ms
```

---

## 🌐 Production Cloud Deployment Guide

The entire platform can be deployed on a **100% Free Tier Cloud Architecture**:

```text
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│     Vercel      │       │     Render      │       │    Supabase     │
│  React 19 SPA   │ ────> │ FastAPI Backend │ ────> │ PostgreSQL 16   │
│ Global Edge CDN │       │ Python Web Svc  │       │  with pgvector  │
└─────────────────┘       └─────────────────┘       └─────────────────┘
```

### 1. Database: Supabase (PostgreSQL + pgvector)
1. Create a free project at [supabase.com](https://supabase.com).
2. Enable `pgvector` in the Supabase SQL Editor:
   ```sql
   CREATE EXTENSION IF NOT EXISTS vector;
   ```
3. Run the included 1-click migration script to copy the complete schema and all 9,479 embeddings:
   ```bash
   python scripts/migrate_to_supabase.py "postgresql://postgres.[REF]:[PASSWORD]@aws-0-[REGION].pooler.supabase.com:6543/postgres"
   ```

### 2. Backend: Render (FastAPI Web Service)
1. Create a new Web Service from your GitHub repository on [render.com](https://render.com).
2. Set Build Command: `pip install -r requirements.txt`
3. Set Start Command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
4. Set Environment Variables: `DATABASE_URL` (Supabase URI) and `GEMINI_API_KEY`.

### 3. Frontend: Vercel (React + Vite SPA)
1. Import repository into [vercel.com](https://vercel.com).
2. Set Root Directory to `frontend`.
3. Set Framework Preset to `Vite`.
4. Deploy! Live updates trigger automatically on push to `main`.

> For complete step-by-step screenshots and configuration, see [`DEPLOYMENT.md`](DEPLOYMENT.md).

---

## 📊 Verified Performance Benchmarks

| Metric | Prior Benchmark | Post-Optimization Benchmark | Improvement Factor |
| :--- | :---: | :---: | :---: |
| **RAG Chat Response Latency** | 120s – 180s (Stalled) | **1.2s – 3.8s** | **~50x Faster** ⚡ |
| **Vite Client Production Build** | ~4.2s | **0.27s (270ms)** | **~15x Faster** |
| **Edge CDN Time-to-First-Byte (TTFB)** | ~1.4s | **0.289s (289ms)** | **~5x Faster** |
| **Client Bundle Size (Gzipped)** | ~180 kB | **107.97 kB** | **40% Reduction** |
| **Multimodal Parser Test Execution** | Manual / None | **93 tests in 0.88s** | **100% Automated** |

---

## 📑 Project Deliverables & Reports Index

| Document / Asset | Description | Source / Generator Script |
| :--- | :--- | :---: |
| [`integration_and_deployment_report.md`](integration_and_deployment_report.md) | Comprehensive Testing, Integration & Deployment Engineering Report | Markdown Source |
| `Integration & Deployment Report PDF` | Formal PDF Integration & Deployment Sign-off Document | [`scripts/generate_pdf_report.py`](scripts/generate_pdf_report.py) |
| `Presentation Slide Deck (.pptx)` | Executive Architecture & Delivery Slide Presentation Deck | [`scripts/generate_presentation_pptx.py`](scripts/generate_presentation_pptx.py) |
| `Presentation Slide Deck (.pdf)` | Slide Deck Presentation in PDF Format | [`scripts/generate_presentation_pdf.py`](scripts/generate_presentation_pdf.py) |
| `Technical Architecture Brief (.pdf)` | Technical Architecture Brief & Evaluation Summary | [`scripts/generate_architecture_pdf.py`](scripts/generate_architecture_pdf.py) |
| [`DEPLOYMENT.md`](DEPLOYMENT.md) | Step-by-Step Free Cloud Deployment Guide (Supabase + Render + Vercel) | Markdown Source |
| [`AI_RAG/HANDOFF.md`](AI_RAG/HANDOFF.md) | Person 5 AI/RAG Architecture Handoff & Specifications | Markdown Source |
| [`docs/database_design.md`](docs/database_design.md) | Complete 12-table PostgreSQL relational schema documentation | Markdown Source |
| [`docs/data_quality_report.md`](docs/data_quality_report.md) | Data Quality validation metrics & audit rules | Markdown Source |
| [`docs/ai_rag_data_contract.md`](docs/ai_rag_data_contract.md) | Input/output contract between DB, API, and RAG components | Markdown Source |

---

## 🔒 Security & Compliance

* **Authorized Ingestion:** The platform processes only authorized course archives and strictly avoids unauthorized web scraping.
* **Credential Isolation:** API keys and database connection strings are managed via `.env` and `.gitignore`, ensuring zero credential exposure in version control.
* **Anti-Hallucination Guardrails:** AI responses must be grounded in verified course segments; unverified or arbitrary citations are actively rejected.
* **Idempotent Ingestion:** The ingestion pipeline can be safely executed repeatedly without creating duplicate records or modifying source archives.

---

## 📜 Quality Assurance Sign-Off

> **Testing, Integration & Deployment Lead Certification:**  
> The **Coursera Multimodal Intelligence Platform** has successfully completed end-to-end regression testing, unit test verification, AI evidence guardrail validation, and production edge deployment testing. All 97 automated tests passed with a 100% success rate, the live Vercel application is fully operational, and the platform is certified for operational deployment.
> 
> * **Status:** ✅ **PRODUCTION READY & CERTIFIED**
> * **Live URL:** [https://coursera-multimodal-intelligence-pl.vercel.app/](https://coursera-multimodal-intelligence-pl.vercel.app/)
