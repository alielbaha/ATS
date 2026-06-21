# Semantic Candidate Matching System

> Academic Project: École Centrale Casablanca (2025-2026) in partnership with Forvis Mazars  

Note : Developed as part of my final year project. Source code was published publicly after the project concluded. 
---

## Overview

This system addresses critical inefficiencies in recruitment processes through advanced Natural Language Processing. Traditional keyword-based systems miss ~40% of qualified candidates and fail to leverage historical applicant pools. Our solution combines state-of-the-art semantic matching with an innovative dormant talent detection module.

1. Semantic Matching Engine
- Sentence-BERT embeddings (all-MiniLM-L6-v2, 384 dimensions)
- Multi-criteria scoring: Semantic (50%) + Skills (20%) + Experience (20%) + Location (10%)
- 0.035s query time for 2000+ candidates (57x faster than target)
- 100% matching accuracy on test cases

2.Talent Rediscovery
- Automatic identification of past applicants who gained relevant experience
- Evolution scoring based on time dormant and profile development


3. Explainable AI
- Transparent score decomposition by criterion
- Natural language justifications (GDPR-compliant)
- Skill gap analysis and hiring recommendations

---

## walkthrough


### Generate Data & Embeddings

```bash
# Generate synthetic dataset (2000 candidates, 50 jobs)
python scripts/generate_data.py

# Generate semantic embeddings
python scripts/generate_embeddings.py
```

### Run Tests

```bash
# Run complete test suite (42 tests, ~40 seconds)
python run_tests.py

# View detailed results
cat logs/test_master_report.txt
```

tests results : ([detailed report](./TEST_RESULTS_ANALYSIS.md))

### Launch Application

```bash
# Web interface at http://localhost:8501
streamlit run app.py
```

---

## System Architecture

### Core Components

| Module | Technology | Function |
|--------|-----------|----------|
| Embeddings | Sentence-BERT (all-MiniLM-L6-v2) | Semantic text representation (384-d) |
| Matching | Multi-criteria weighted scoring | Candidate ranking with explainability |
| Dormant Detection | Temporal + semantic analysis | Past applicant rediscovery |
| Filtering | Rule-based constraints | Location, experience, skills filtering |
| Explainability | NLG + score decomposition | Transparent decision support |

---

## Performance & Testing

### Performance Metrics (Intel i5-1145G7, 16GB RAM)

| Metric | Target | Actual | 
|--------|--------|--------|
| Query Time | < 2.0s | **0.035s** |  
| Batch Processing | > 5 jobs/s | **27.57 jobs/s** |  
| Concurrent Queries | > 10 q/s | **28.38 q/s** | 
| Memory Usage | < 1 GB | **600 MB** | 
| Matching Accuracy | > 80% | **100%** | 

### Test Coverage

Comprehensive automated testing with **42 tests** across 4 suites:

| Test Suite | Tests | Coverage | Status |
|------------|-------|----------|--------|
| Data Quality | 10 | 95% |  10/10 passed |
| Embeddings | 7 | 90% |  7/7 passed |
| Matching Engine | 17 | 95% |  17/17 passed |
| Integration | 8 | 85% |  8/8 passed |
| **TOTAL** | **42** | **91%** |  **42/42 passed** |


Full test results: [TEST_RESULTS_ANALYSIS.md](./TEST_RESULTS_ANALYSIS.md)

---

---

## Usage

### Python API

```python
from src.matching_engine import MatchingEngine
import json

# Load data
with open('data/jobs.json') as f:
    jobs = json.load(f)

# Initialize engine
engine = MatchingEngine()

# Find matches
matches = engine.find_matches(jobs[0]['job_id'], top_k=10)

# Results with explanations
for match in matches:
    print(f"{match['name']}: {match['final_score']:.2f}")
    print(f"  Reasoning: {match['explanation']}")
```

### Web Interface

Navigate through 4 modules:
1. Candidate Matching: Real-time semantic search for job positions
2. Dormant Talent: Rediscover past applicants for new opportunities
3. System Stats: Performance metrics and data insights
4. About: Technical documentation and team information

---

## Project Structure

```
semantic-matching-ats/
├── config.py                    
├── app.py                       
├── run_tests.py                 
├── data/
│   ├── candidates.json         # Candidate profiles
│   ├── jobs.json               # Job postings
│   ├── applications.json       # Application history
│   ├── candidate_embeddings.npy # Semantic vectors (2000×384)
│   └── job_embeddings.npy     
├── src/
│   ├── data/
│   │   └── synthetic_generator.py
│   ├── models/
│   │   └── embedding_engine.py
│   ├── search/
│   │   ├── matching_engine.py
│   │   └── dormant_detector.py
│   └── explainability/
│       └── explainer.py
├── scripts/
│   ├── generate_data.py        
│   └── generate_embeddings.py  
├── tests/
│   ├── test_data_quality.py    # Data validation (10 tests)
│   ├── test_embeddings.py      # Embedding quality (7 tests)
│   ├── test_matching_engine.py # Matching logic (17 tests)
│   ├── test_integration.py     # End-to-end (8 tests)
│   └── test_utils.py          
└── logs/
    └── test_*.txt              
```

---

## Academic Context

Institution: École Centrale Casablanca  
Program: Engineering Cycle - Option Data Science  
Academic Year: 2025-2026  
Project Type: Industry Partnership 
Partner: Forvis Mazars (International Audit & Advisory)  
Duration: 3 months (January - March 2025)
