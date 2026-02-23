# ⚡ Multimodal IDP Pipeline: LLM-Driven Invoice ETL

[Live Deployment / Repository](URL_DO_POPRAWIENIA)

## 🎯 Architecture Overview & Business Objective
An Intelligent Document Processing (IDP) system engineered to automate unstructured financial data extraction (PDF/IMG) and reconcile it with internal relational systems. The pipeline replaces fragile, heuristic-based OCR with a multimodal Generative AI engine constrained by strict data schemas. This guarantees deterministic structuring of invoice data for downstream integrity checks and ERP ingestion.

## 🛠 Tech Stack & Ecosystem
| Component | Technology |
| :--- | :--- |
| **LLM Orchestration** | `instructor` (Strict JSON Schema Enforcement via Pydantic) |
| **Multimodal Engine** | `OpenAI GPT-4o` (Zero-shot vision extraction) |
| **Data Engineering** | `Pandas` (Vectorized Relational Merges) |
| **Data Validation** | `Pydantic v2` (Type coercion & logical validation) |
| **Storage & Lookup** | `SQLite` (Client Data), `CSV` (Master Product Catalogs) |
| **Ingestion** | `pdf2image`, Base64 encoding |

## 🏗 Core Pipeline Mechanics
The system executes a robust Extract-Transform-Load (ETL) workflow prioritizing data integrity:

### 1. Multimodal Ingestion & Schema Enforcement
Bypassing traditional OCR pipelines, the system utilizes `gpt-4o` combined with the `instructor` wrapper.
* **Deterministic Output:** Unstructured visual data is directly mapped into a highly typed `InvoiceInfo` Pydantic model, eliminating regex-based parsing errors.
* **Automated Preprocessing:** Native conversion of multi-format inputs (PDFs via `pdf2image`) into normalized Base64 payloads for the Vision API.

[Image of an Intelligent Document Processing pipeline using multimodal LLM for data extraction, schema validation, and database ingestion]

### 2. Cross-Source Deterministic Reconciliation
The most critical tier of the pipeline. Once the probabilistic LLM extraction is validated by Pydantic, the payload enters a deterministic `Pandas` computation engine:
* **Relational Joins:** Executing vectorized merges between extracted invoice data and the SQLite client database.
* **Master Data Lookup:** Hash-matching extracted SKUs/prices against the authoritative product catalog (`zad_domowe__products.csv`).
* **Automated Auditing:** Computing the `total_price` tensor and dynamically flagging price discrepancies (`ceny_sie_zgadzaja` boolean flag) in $O(N)$ time complexity.

### 3. Stateful Artifact Orchestration
The pipeline implements a robust, directory-based Finite State Machine for file lifecycle management, ensuring no data is processed twice or lost silently:
* `raw/`: Ingestion queue.
* `processed/`: Successfully reconciled and audited payloads.
* `error/`: Quarantined artifacts failing Pydantic validation or lacking strict LLM schema adherence.