# AssetOpsBench: Agentic AI Challenge (CODS-2025)

Benchmark for multi-agent AI systems in industrial operations — covering predictive maintenance, fault diagnosis, task allocation, technician assist, and asset logging.  

This README gives **step-by-step setup instructions** so you can:  
- Run the starter kit locally.  
- Test the baseline codebase.  
- Develop and extend workflows.  
- Prepare submissions for Codabench.  

---

## 📑 Table of Contents
1. [Overview](#overview)  
2. [Prerequisites](#prerequisites)  
3. [Setup Instructions](#setup-instructions)  
   - [Clone Repository](#a-clone-repository)  
   - [Install Core Tools](#b-install-core-tools)  
   - [Create Environment](#c-create-environment)  
   - [Install Dependencies](#d-install-dependencies)  
   - [Run with Docker](#e-run-with-docker-recommended)  
   - [Download Datasets](#f-download-datasets)  
4. [Running Baselines](#running-baselines)  
5. [Development Notes](#development-notes)  
6. [Codabench Submission](#codabench-submission)  
7. [Troubleshooting](#troubleshooting)  

---

## 🔎 Overview
AssetOpsBench provides:  
- **140+ scenarios** across asset types (time-series, logs, documents).  
- **Two tracks**:  
  1. **Track 1: Planning-oriented orchestration**  
  2. **Track 2: Execution-oriented workflows**  
- **Starter Kit** with baselines, submission templates, and Docker orchestration.  
- Evaluation on **LLaMA-3-70B** (fixed model at evaluation).  

Official pages:  
- [Challenge Website](https://sites.google.com/view/assetopsbench-challenge/home)  
- [Codabench Competition](https://www.codabench.org/competitions/10206/#/pages-tab)  
- [GitHub Starter Kit](https://github.com/IBM/AssetOpsBench)  

---

## ⚙️ Prerequisites
Before setup, ensure you have:  
- **Git** (clone repo).  
- **Docker + Docker Compose** (recommended — runs the benchmark stack).  
- **Python ≥3.10** (for local dev).  
- **Conda** (preferred) or `venv`.  
- **5–20 GB disk space** for Docker images + datasets.  
- (Optional) **GPU + large RAM** if running large LLMs locally.  

---

## 🛠️ Setup Instructions

### A. Clone Repository
```bash
git clone https://github.com/IBM/AssetOpsBench.git
cd AssetOpsBench
```
### B. Install Core Tools
- Install Docker Desktop(Windows/Mac) or Docker Engine + Compose (Linux).
- Install Python 3.10+ and Conda (recommended).

### C. Create Environment
Using Conda:
```bash
conda create -n assetopsbench python=3.12 -y
conda activate assetopsbench
```
Using Venv:
```bash
python3 -m venv venv
source venv/bin/activate
```

### D. Install Dependencies
Check for requirements files in repo:
```bash
ls | grep -E "requirements|basic_require|extra_require"
```
Install: 
```bash
pip install -r requirements.txt
# or, if layered:
pip install -r basic_requirements.txt
pip install -r extra_requirements.txt   # optional extras
```

### E. Run with Docker (Recommended)
The repo provides Docker orchestration for reproducibility.
```bash
# make entrypoint executable
chmod +x benchmark/entrypoint.sh

# build and start stack
docker-compose -f benchmark/docker-compose.yml build
docker-compose -f benchmark/docker-compose.yml up
```
Using prebuilt images(faster):
```bash
docker pull quay.io/assetopsbench/assetopsbench-basic
docker pull quay.io/assetopsbench/assetopsbench-extra
```
Then adjust docker-compose.yml to use pulled images.

### F. Download Datasets
From HuggingFace:
```bash
from datasets import load_dataset
ds = load_dataset("ibm-research/AssetOpsBench")
```
Or use the scenarios/ folder from the repo.

---

▶️ Running Baselines
1. Start stack:
```bash
docker-compose -f benchmark/docker-compose.yml up
```
2. Run baseline example:
```bash
./benchmark/entrypoint.sh
```
3. Or run locally (non-Docker):
```bash
python -m src.run_demo --scenario scenarios/sample_scenario.jsonl
```

---

💻 Development Notes
LLMs:
- Local dev does not require LLaMA-3-70B.
- Use smaller models (LLaMA-2-7B, Mistral-7B, OpenAI/GPT APIs) or mock responses.
Templates: 
- Modify only TODO sections in submission templates.
Config: 
- Keep model keys & endpoints configurable via .env or config files.

---

🚀 Codabench Submission
- Register for the competition
- Prepare submission using provided templates (submission_template_track1.json, submission_template_track2.json).
- Zip predictions or bundle code as required.
- Submit via My Submissions tab on Codabench.
- Monitor logs & leaderboard.
Limits:
- Max 50 submissions per team in Phase 1.
- Strict daily cap enforced.

---

🐞 Troubleshooting
- Docker build OOM → use assetopsbench-basic prebuilt image.
- Missing dependencies → check both basic_requirements.txt and extra_requirements.txt.
- LLM too heavy → mock/stub locally, rely on Codabench for true evaluation.
- Codabench submission errors → check logs; ensure JSON structure matches template.