# Data Science & Business Analytics Portfolio

This repository serves as the central documentation hub for my analytics projects. I utilize a Documentation-as-Code approach, building an interactive site with MkDocs and automating deployments via CI/CD pipelines.

## Live Deployment
The full, compiled project documentation is available here: https://c8nraad.github.io/Portfolio-Data-Science/

## Technical Stack
* **Documentation:** MkDocs (Material theme)
* **Data Analysis:** Python (Pandas, NumPy)
* **Automation:** GitHub Actions (CI/CD pipeline triggered on push)
* **Integrations:** Multimodal OCR Pipeline, GPT-4o API, SQLite

## Key Projects (Case Studies)

### 1. AI Invoice Intelligence (Multimodal ETL)
An advanced Extract-Transform-Load data pipeline.
* **Problem:** Unstructured data extraction from PDF/JPG invoices.
* **Solution:** Leveraging the GPT-4o vision model to extract and validate payloads directly into strictly typed Python objects (`Pydantic`).
* **Data Engineering:** Reconciling extracted payloads with a SQLite client database using vectorized Pandas joins.

### 2. Insurance Risk Analysis
Processing and sanitizing business datasets.
* **Technologies:** Pandas, Matplotlib
* **Business Value:** Identification of key risk factors driving portfolio profitability and premium optimization.

## Repository Structure
* `docs/` - Markdown source files containing project specifications.
* `mkdocs.yml` - Main site configuration and routing.
* `.github/workflows/` - CI/CD deployment automation scripts.