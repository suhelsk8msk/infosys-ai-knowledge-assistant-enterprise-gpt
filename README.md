# 🏢 Infosys: AI Knowledge Assistant (Enterprise GPT)

[![Live Application](https://img.shields.io/badge/Live%20App-Vercel%20Deployment-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://infosys-ai-knowledge-assistant-ente-chi.vercel.app/login)
[![GitHub Repository](https://img.shields.io/badge/GitHub-Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/suhelsk8msk/infosys-ai-knowledge-assistant-enterprise-gpt)
![Python](https://img.shields.io/badge/Python-3.11%2B-3776AB?style=flat&logo=python&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-14.2%20(App%20Router)-000000?style=flat&logo=next.js&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.115%2B-009688?style=flat&logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16%20%2B%20pgvector-336791?style=flat&logo=postgresql&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Google%20Gemini-API%20(1536--dim)-4285F4?style=flat&logo=google&logoColor=white)
![PyMuPDF](https://img.shields.io/badge/PyMuPDF-PDF%20Parsing-E25A1C?style=flat)
![Cloudflare R2](https://img.shields.io/badge/Cloudflare%20R2-Object%20Storage-F38020?style=flat&logo=cloudflare&logoColor=white)
![Render](https://img.shields.io/badge/Render-Backend%20Hosting-46E3B7?style=flat&logo=render&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-Frontend%20Hosting-000000?style=flat&logo=vercel&logoColor=white)

> **Enterprise-Grade Retrieval-Augmented Generation (RAG) & Document Intelligence Platform** designed for corporate knowledge workers. Enables secure multi-department document ingestion, sub-second semantic retrieval using 1,536-dimensional vector embeddings, grounded AI synthesis with strict anti-hallucination guardrails, and transparent inline source citations with Role-Based Access Control (RBAC).

---

> [!NOTE]
> **Independent Portfolio Project Disclaimer**:  
> This platform was engineered by an independent 7-member engineering team as an enterprise-grade technical demonstration and capstone project. It is **not** officially affiliated with, endorsed by, or sponsored by Infosys Limited.

---

## 🔗 Quick Links

- **🌐 Live Production Application**: [https://infosys-ai-knowledge-assistant-ente-chi.vercel.app/login](https://infosys-ai-knowledge-assistant-ente-chi.vercel.app/login)
- **💻 GitHub Source Code**: [https://github.com/Ujwal-Bamb/infosys-ai-knowledge-assistant-enterprise-gpt.git](https://github.com/suhelsk8msk/infosys-ai-knowledge-assistant-enterprise-gpt)
- https://github.com/suhelsk8msk/infosys-ai-knowledge-assistant-enterprise-gpt
- **⚡ Backend API Service**: Deployed on [Render](https://render.com) (FastAPI + Uvicorn)
- **🗄️ Vector Database**: PostgreSQL with `pgvector` extension (Cosine distance metric)
- **📦 Document Storage**: Cloudflare R2 S3-Compatible Object Store

---

## 📸 Screenshots

> *Application visual walkthrough showcasing role-based authentication, grounded question-answering with inline document citations, document ingestion, and distributed cloud architecture:*

| Role-Based Login & Authentication | Grounded Enterprise Q&A with Inline Citations |
| :---: | :---: |
| ![Login UI](docs/screenshots/login_ui.png) | ![Chat Citations UI](docs/screenshots/chat_citations_ui.png) |
| *JWT-secured multi-role authentication (Admin / Employee / Employer)* | *Strictly grounded conversational Q&A displaying document codes, page numbers, and similarity scores* |

| Document Upload, PyMuPDF Parsing & Ingestion | End-to-End Cloud Deployment & Pipeline |
| :---: | :---: |
| ![PDF Ingestion UI](docs/screenshots/pdf_ingestion_ui.png) | ![Deployment Architecture](docs/screenshots/deployment_architecture.png) |
| *Admin-governed multi-department PDF upload, PyMuPDF chunking, and pgvector embedding* | *Production cloud integration across Vercel, Render, Cloudflare R2, and Managed PostgreSQL* |

---

## 🏗️ System Architecture & RAG Pipeline

```mermaid
flowchart TD
    subgraph ClientLayer["Frontend Client (Vercel)"]
        UI["Next.js 14 App Router (React 18 + Tailwind CSS)"]
        AuthCtx["JWT Auth Context & Role Guard"]
        ChatUI["Interactive Chat & Inline Citation Renderer"]
        DocUI["Admin Document Ingestion Panel"]
    end

    subgraph APILayer["API Gateway & Microservices (Render)"]
        FastAPI["FastAPI 0.115+ Application"]
        AuthRouter["Auth Router (/auth/login, /auth/me)"]
        DocRouter["Document Router (/documents/upload)"]
        QueryRouter["Query Router (/query)"]
        AnalyticsRouter["Analytics Router (/analytics/dashboard-stats)"]
    end

    subgraph ProcessingLayer["Ingestion & Parsing Engine"]
        PyMuPDF["PyMuPDF fitz Text Extractor"]
        TextChunker["Text Chunker (Size: 850 chars, Overlap: 150 chars)"]
        GeminiEmbed["Google Gemini Embedding API (gemini-embedding-001, 1536-dim)"]
    end

    subgraph StorageLayer["Persistence & Vector Data Stores"]
        CloudflareR2["Cloudflare R2 Object Storage (boto3 S3 API)"]
        PostgresDB[("PostgreSQL 16 Relational Store")]
        PGVector[("pgvector Extension (Cosine Distance <= 0.40 / Sim >= 0.60)")]
    end

    subgraph LLMLayer["AI Synthesis & Guardrails"]
        GeminiLLM["Google Gemini 3.5 Flash Lite"]
        AntiHallucination["Strict Context-Bound Synthesis Engine"]
    end

    %% Client Interactions
    UI -->|HTTPS / JWT Bearer| FastAPI
    FastAPI --> AuthRouter
    FastAPI --> DocRouter
    FastAPI --> QueryRouter
    FastAPI --> AnalyticsRouter

    %% Document Ingestion Flow
    DocRouter -->|Presign / Store Raw PDF| CloudflareR2
    DocRouter --> PyMuPDF
    PyMuPDF --> TextChunker
    TextChunker --> GeminiEmbed
    GeminiEmbed -->|Batch Vector Embeddings| PGVector
    DocRouter -->|Store Metadata| PostgresDB

    %% Query & Retrieval Flow
    QueryRouter -->|1. Generate Query Vector| GeminiEmbed
    GeminiEmbed -->|2. 1536-dim Query Vector| PGVector
    PGVector -->|3. Top-k Chunks (Sim >= 0.60)| QueryRouter
    QueryRouter -->|4. Context + Strict Prompt| AntiHallucination
    AntiHallucination --> GeminiLLM
    GeminiLLM -->|5. Grounded Answer + Citations| ChatUI
```

### End-to-End Execution Flow

1. **Document Ingestion & Object Storage**:
   - An authorized Administrator uploads corporate PDF policies or documentation via the `/documents/upload` endpoint.
   - The original binary PDF is safely streamed into **Cloudflare R2** via `boto3` utilizing S3-compatible endpoints, ensuring durable, high-throughput cloud storage.
2. **Deterministic Text Extraction & Semantic Chunking**:
   - **PyMuPDF** (`fitz`) parses each page individually, preserving exact page numbering metadata (`page_number`).
   - The custom `text_chunker` slices text into sliding windows of **850 characters** with a **150-character overlap**, respecting natural whitespace boundaries to avoid splitting corporate entities or technical terms.
3. **1,536-Dimensional Vector Embedding**:
   - Chunks are vectorized using Google Gemini's embedding model (`gemini-embedding-001`) with explicit `RETRIEVAL_DOCUMENT` task typing and dimensionality set to **1,536**.
   - Embeddings and metadata are persisted in PostgreSQL using the `pgvector` `Vector(1536)` column type.
4. **Vector Retrieval with Similarity Guardrails**:
   - When an authenticated user submits a question through `/query` (with an optional department filter), the query is converted into a 1,536-dimensional vector using `task_type="RETRIEVAL_QUERY"`.
   - `pgvector` performs cosine distance calculation (`DocumentChunk.embedding.cosine_distance(query_vector)`).
   - A strict similarity threshold of **`0.60`** (`distance <= 0.40`) filters out irrelevant context, retrieving the top $K=4$ most relevant chunks.
5. **Grounded Answer Synthesis & Source Citations**:
   - The retrieved chunks are formatted into structured source blocks: `[Source N] Document: <title> Page: <p> Content: <text>`.
   - **Google Gemini 3.5 Flash Lite** processes the prompt under zero-hallucination constraints: if the context does not contain verifiable evidence, it outputs a calibrated refusal rather than speculating.
   - The response delivers the synthesized answer alongside structured source citations (document code, title, page number, and similarity score) directly rendered in the Next.js UI.

---

## 👥 7-Member Engineering Team & Module Ownership

This platform was built through collaborative cross-functional engineering, with clear ownership across each architectural boundary:

| Team Member | Engineering Role | Core Responsibilities & Modules Delivered |
|:---|:---|:---|
| **Ujwal Bamb** | **Team Lead \| System Integration & Cloud Deployment** | • End-to-end cloud infrastructure orchestration (Vercel, Render, Cloudflare R2, PostgreSQL + pgvector)<br>• Architecture design, cross-module schema alignment, and backend-frontend integration<br>• Production release management, CORS security, and deployment verification |
| **Gaurav** | **Frontend & User Experience Engineer** | • Next.js 14 App Router frontend architecture and responsive component hierarchy<br>• Interactive Chat UI with streaming response states and dynamic source citation cards<br>• Login, registration, document management dashboards, and profile UI |
| **Anamika** | **Authentication & Database Architect** | • Role-Based Access Control (RBAC) modeling (`Admin`, `Employee`, `Employer`)<br>• Password security with `passlib` bcrypt hashing and JWT token issuance/verification<br>• PostgreSQL relational schema design (`users`, `roles`, `documents`, `query_history`) |
| **Manapriti** | **Document Ingestion & Text Chunking Engineer** | • High-fidelity PDF document parsing pipeline using `PyMuPDF` (`fitz`)<br>• Space-aware sliding window text chunking algorithm (850-character window / 150-character overlap)<br>• Metadata preservation ensuring page-level traceability across all chunks |
| **Siddhant** | **Embeddings & Vector Retrieval Engineer** | • Integration of Google Gemini embedding pipeline (`gemini-embedding-001`, 1,536 dimensions)<br>• `pgvector` database indexing and query vector embedding (`RETRIEVAL_QUERY`)<br>• Cosine distance optimization with rigorous similarity thresholding ($\text{similarity} \ge 0.60$) |
| **Satyajeet** | **AI Answer Generation & API Integration Engineer** | • Core FastAPI `/query` endpoint implementation and request validation (`Pydantic`)<br>• Zero-hallucination prompt engineering for Google Gemini 3.5 Flash Lite synthesis<br>• Structured source citation packaging and query history audit logging |
| **Mohammad Suhel** | **Testing & Quality Assurance Engineer** | • Comprehensive test suite authoring across access control, auth, and ingestion<br>• RAG retrieval accuracy verification, cosine threshold edge-case validation<br>• Integration testing (`pytest`, `httpx`), error handling, and end-to-end QA validation |

---

## 📁 Repository Structure

```text
infosys-ai-knowledge-assistant-enterprise-gpt/
├── .env.example                       # Reference environment variables
├── .gitignore                         # Git exclusion rules
├── README.md                          # Master documentation & project guide
├── enable_pgvector.py                 # Standalone script to enable PostgreSQL vector extension
│
├── ai_workflows/                      # AI & LLM synthesis pipelines
│   ├── grounded_synthesis/
│   │   └── answer_service.py          # Gemini 3.5 Flash Lite prompt & anti-hallucination logic
│   └── rag_retrieval/
│       └── retrieval_service.py       # pgvector cosine similarity search (threshold: 0.60)
│
├── backend/                           # FastAPI application & services
│   ├── requirements.txt               # Backend Python dependencies
│   ├── api/
│   │   └── main.py                    # API entry point & routing (/auth, /documents, /query)
│   ├── auth/                          # Authentication & RBAC
│   │   ├── dependencies.py            # FastAPI auth dependencies & token extraction
│   │   ├── permissions.py             # Role validation (Admin, Employee, Employer)
│   │   ├── schemas.py                 # Auth request/response schemas
│   │   ├── security.py                # Bcrypt password hashing
│   │   ├── service.py                 # User creation & credential verification
│   │   └── token.py                   # JWT creation & decoding logic
│   ├── database/                      # Database models & migrations
│   │   ├── connection.py              # SQLAlchemy engine & session management
│   │   ├── dependencies.py            # get_db session dependency
│   │   ├── init_db.py                 # Table creation script
│   │   ├── models.py                  # User, Role, Document, DocumentChunk, QueryHistory models
│   │   ├── seed_admin.py              # Secure interactive Admin account provisioning script
│   │   └── seed_roles.py              # Role seeder (Admin, Employee, Employer)
│   └── services/                      # Business logic layer
│       ├── document_schemas.py        # Pydantic schemas for document metadata
│       ├── document_service.py        # Cloudflare R2 upload, PyMuPDF parsing, and chunk storage
│       ├── embedding_service.py       # Google Gemini 1536-dim embedding generator
│       └── query_schemas.py           # Query and citation schemas
│
├── frontend/                          # Next.js 14 Web Application
│   ├── package.json                   # Frontend dependencies (Next 14, React 18, Tailwind CSS)
│   ├── tsconfig.json                  # TypeScript compiler configuration
│   ├── tailwind.config.ts             # Tailwind design system tokens
│   ├── app/                           # App Router pages
│   │   ├── layout.tsx                 # Root layout & font definitions
│   │   ├── globals.css                # Global stylesheet & Tailwind directives
│   │   ├── (auth)/                    # Authentication route group
│   │   │   ├── login/                 # User login page
│   │   │   └── register/              # User registration page
│   │   └── (dashboard)/               # Protected dashboard route group
│   │       ├── page.tsx               # Analytics & dashboard overview
│   │       ├── chat/                  # Conversational RAG knowledge assistant
│   │       ├── documents/             # Document management & status
│   │       ├── upload/                # Admin-only PDF upload interface
│   │       ├── recent-queries/        # User query audit log
│   │       └── profile/               # User profile & security settings
│   ├── components/                    # Reusable React components
│   │   ├── chat/                      # SourceCard, SuggestedQuestions
│   │   ├── layout/                    # Header, Sidebar navigation
│   │   └── ui/                        # Button, Badge primitives
│   ├── context/                       # AuthContext & state providers
│   ├── lib/                           # API fetch client & utility helpers
│   └── types/                         # TypeScript interfaces
│
├── ingestion_pipeline/                # Document processing modules
│   ├── document_parsing/
│   │   └── pdf_parser.py              # PyMuPDF text & page extractor
│   ├── chunking/
│   │   └── text_chunker.py            # Sliding window text chunker (850 / 150)
│   └── embedding_jobs/
│       └── embedding_job.py           # Batch chunk embedding orchestrator
│
├── database_backup/                   # Database dump & migration scripts
│   ├── infosys_ai_backup.dump         # PostgreSQL schema & data backup
│   └── users_to_migrate.csv           # User migration dataset
│
├── docs/                              # Project documentation & specs
│   └── screenshots/                   # Application screenshots for portfolio display
│       ├── login_ui.png
│       ├── chat_citations_ui.png
│       ├── pdf_ingestion_ui.png
│       └── deployment_architecture.png
│
├── deployment/                        # Deployment setup notes (Vercel, Render)
│   ├── environment_setup.md
│   ├── render_notes.md
│   └── vercel_notes.md
│
└── tests/                             # Comprehensive automated test suite
    ├── access_control_tests/          # RBAC, password security & document auth tests
    ├── ai_output_tests/               # Gemini grounded synthesis output tests
    ├── functional_tests/              # Login, dashboard analytics & history tests
    ├── ingestion_tests/               # PyMuPDF, chunker & embedding storage tests
    └── retrieval_tests/               # pgvector cosine search & threshold tests
```

---

## 🛠️ Local Setup & Execution Guide

Follow the steps below to run both the FastAPI backend and the Next.js frontend in your local development environment:

### 1. Prerequisites

- **Python**: 3.10 or 3.11+
- **Node.js**: 18.x or 20.x+ (`npm` or `pnpm`)
- **PostgreSQL**: 15+ with the `pgvector` extension installed
- **Google Gemini API Key**: From [Google AI Studio](https://aistudio.google.com/)
- **Cloudflare R2 Bucket**: Or local filesystem storage fallback

---

### 2. Environment Configuration

Create a `.env` file in the repository root based on `.env.example`:

```bash
cp .env.example .env
```

Fill in the required configuration variables:

```ini
# Core Application
APP_NAME="Infosys AI Knowledge Assistant"
APP_ENV=development
DEBUG=true
API_V1_PREFIX=/api
CORS_ORIGINS=http://localhost:3000,https://infosys-ai-knowledge-assistant-ente-chi.vercel.app

# Database & Vector Store
DATABASE_URL=postgresql+psycopg://postgres:your_password@localhost:5432/infosys_ai

# Security & Authentication
SECRET_KEY=generate_a_64_character_hex_secret_key_here
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=60

# Google Gemini AI & Embeddings
GEMINI_API_KEY=your_gemini_api_key_here
LLM_MODEL=gemini-3.5-flash-lite
EMBEDDING_MODEL=gemini-embedding-001
EMBEDDING_DIMENSION=1536

# Cloudflare R2 Object Storage
R2_ENDPOINT_URL=https://<account_id>.r2.cloudflarestorage.com
R2_ACCESS_KEY_ID=your_r2_access_key_id
R2_SECRET_ACCESS_KEY=your_r2_secret_access_key
STORAGE_BUCKET=infosys-knowledge-docs
MAX_UPLOAD_SIZE_MB=10
```

For the frontend, create `frontend/.env.local`:

```ini
NEXT_PUBLIC_API_URL=http://localhost:8000
```

---

### 3. Backend Setup & Database Initialization

1. **Create and Activate Virtual Environment**:
   ```bash
   python -m venv venv
   # On Windows:
   venv\Scripts\activate
   # On Linux/macOS:
   source venv/bin/activate
   ```

2. **Install Backend Dependencies**:
   ```bash
   pip install -r backend/requirements.txt
   ```

3. **Initialize Database & pgvector Extension**:
   ```bash
   # Enable pgvector extension in PostgreSQL
   python enable_pgvector.py

   # Create all tables (roles, users, documents, document_chunks, query_history)
   python -m backend.database.init_db

   # Seed default roles (Admin, Employee, Employer)
   python -m backend.database.seed_roles

   # Provision initial Administrator account securely (prompts for email & password)
   python -m backend.database.seed_admin
   ```

4. **Launch FastAPI Backend Server**:
   ```bash
   uvicorn backend.api.main:app --reload --host 0.0.0.0 --port 8000
   ```
   The backend API is now accessible at `http://localhost:8000`.  
   Interactive Swagger documentation is available at `http://localhost:8000/docs`.

---

### 4. Frontend Setup & Execution

1. **Navigate to Frontend Directory & Install Dependencies**:
   ```bash
   cd frontend
   npm install
   ```

2. **Run Next.js Development Server**:
   ```bash
   npm run dev
   ```
   Open [http://localhost:3000](http://localhost:3000) in your browser to access the application.

---

### 5. Running Automated Tests

The repository includes a comprehensive `pytest` suite validating access control, vector search, ingestion, and AI outputs:

```bash
# Run all tests
pytest tests/ -v

# Run specific test suites
pytest tests/access_control_tests/ -v
pytest tests/retrieval_tests/ -v
pytest tests/ingestion_tests/ -v
pytest tests/functional_tests/ -v
```

---

## 📡 Key API Endpoints Reference

| Method | Endpoint | Access Role | Description |
|:---|:---|:---|:---|
| `GET` | `/health` | Public | System health check and database connectivity probe |
| `POST` | `/auth/login` | Public | Authenticates user; returns JWT Bearer token & role info |
| `POST` | `/auth/register` | Public / Admin | Registers a new user account with assigned role |
| `GET` | `/auth/me` | Authenticated | Retrieves current authenticated user profile & permissions |
| `POST` | `/documents/upload` | **Admin Only** | Multipart PDF upload; streams to R2, chunks with PyMuPDF, and generates 1536-dim pgvector embeddings |
| `GET` | `/documents` | Authenticated | Lists all registered organizational documents with metadata |
| `DELETE` | `/documents/{doc_id}` | **Admin Only** | Deletes document record, associated chunks, and R2 object |
| `POST` | `/query` | Authenticated | Core RAG endpoint: embeds query, retrieves top-k chunks ($cosine \le 0.40$), and synthesizes grounded answer with citations |
| `GET` | `/queries/history` | Authenticated | Retrieves user-specific past questions, answers, and timestamps |
| `DELETE` | `/queries/history` | Authenticated | Clears user-specific query audit history |
| `GET` | `/analytics/dashboard-stats`| Authenticated | Returns real-time counts of documents, chunks, queries, and active departments |

---

## 👨‍💻 Project Lead & Contact

**Ujwal Bamb**  
*Team Lead \| Machine Learning & Full-Stack Systems Integration*  
- **LinkedIn**: [linkedin.com/in/ujwal-bamb-276b04330](https://www.linkedin.com/in/ujwal-bamb-276b04330)  
- **GitHub**: [github.com/Ujwal-Bamb](https://github.com/Ujwal-Bamb)  
- **Portfolio Repository**: [infosys-ai-knowledge-assistant-enterprise-gpt](https://github.com/Ujwal-Bamb/infosys-ai-knowledge-assistant-enterprise-gpt.git)
