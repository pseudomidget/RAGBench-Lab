# RAGBench-Lab
A Research Framework for Evaluating Retrieval-Augmented Generation

RAGBench Lab is a research-oriented framework for investigating how retrieval strategies influence the quality, relevance, and reliability of evidence used by Retrieval-Augmented Generation (RAG) systems.

The project compares traditional keyword-based retrieval, dense vector retrieval, hybrid search, and cross-encoder reranking on scientific evidence retrieval tasks. It aims to identify the trade-offs between retrieval quality, computational cost, and answer groundedness through controlled experiments.

## Research Question

**How do retrieval strategies and ranking techniques affect evidence retrieval quality, answer groundedness, and computational efficiency in scientific question-answering systems?**

## Objectives

- Compare sparse, dense, and hybrid retrieval methods under consistent evaluation conditions.
- Investigate the effect of reranking on the relevance of retrieved evidence.
- Conduct ablation studies to understand how retrieval configurations affect model performance.
- Evaluate retrieval quality using standard information retrieval metrics.
- Analyze failure cases and measure the trade-off between retrieval quality and latency.
- Develop a reproducible evaluation pipeline and interactive results dashboard.

## Research Hypotheses

- **H1:** Dense retrieval can improve semantic matching for queries that have limited keyword overlap with relevant documents.
- **H2:** Hybrid retrieval can improve evidence coverage by combining lexical and semantic matching.
- **H3:** Cross-encoder reranking can improve the relevance of highly ranked passages at the cost of additional computational overhead.
- **H4:** Improved retrieval quality does not necessarily guarantee more factually correct or better-grounded generated answers.

These hypotheses will be evaluated empirically rather than assumed to be true.

## Dataset

**SciFact — Scientific Claim Verification**

The project uses the SciFact dataset through the BEIR information retrieval benchmark.

Dataset: https://github.com/beir-cellar/beir

SciFact contains scientific claims, research abstracts, and evidence relevance annotations. It provides a useful benchmark for studying whether retrieval systems identify scientific documents relevant to a claim.

The initial implementation will focus on evidence retrieval. Answer generation and groundedness evaluation will be added as a separate stage.

## Methodology

### 1. Data Preparation
- Load the SciFact corpus, queries, and relevance judgments.
- Normalize text and preserve document identifiers.
- Prepare consistent evaluation inputs across retrieval methods.

### 2. Retrieval Baselines
Implement and compare:

- **BM25:** Keyword-based sparse retrieval.
- **Dense Retrieval:** Semantic search using pretrained text embeddings.
- **Hybrid Retrieval:** Combination of sparse and dense retrieval using a rank-fusion strategy.
- **Cross-Encoder Reranking:** Reorder retrieved candidates based on query-document relevance.

### 3. Experimental Evaluation

| Experiment | Investigation |
|---|---|
| Baseline comparison | Sparse vs. dense retrieval |
| Hybrid retrieval | Effect of combining lexical and semantic search |
| Reranking | Effect of reranking on top-ranked evidence |
| Parameter ablation | Effect of top-k values and retrieval configurations |
| Efficiency analysis | Quality, latency, and computational cost |
| Generation evaluation | Relationship between retrieved evidence and answer groundedness |

### 4. Evaluation Metrics

**Information retrieval**
- Recall@k
- nDCG@k
- Mean Reciprocal Rank (MRR)

**Efficiency**
- Query latency
- Runtime and memory usage where measurable

**Generated answers**
- Answer correctness against reference answers
- Faithfulness to retrieved evidence
- Context precision and context recall, where applicable

Retrieval and generation metrics will be reported separately because finding relevant evidence does not guarantee that a language model will use it correctly.

### 5. Error Analysis

Analyze queries for which retrieval methods disagree or fail to retrieve relevant evidence. Investigate possible causes such as terminology mismatch, ambiguous claims, weak semantic matching, and incorrect ranking.

## Technology Stack

- **Programming:** Python
- **Data Analysis:** NumPy, Pandas
- **Information Retrieval:** BM25, Sentence Transformers
- **Machine Learning:** Scikit-learn, Hugging Face Transformers
- **Vector Search:** FAISS, where appropriate
- **Evaluation:** BEIR metrics and custom evaluation scripts
- **Experiment Tracking:** MLflow or structured experiment logs
- **Visualization:** Matplotlib, Streamlit
- **Version Control:** Git and GitHub

## System Architecture

1. Dataset ingestion and preprocessing
2. Sparse and dense retrieval
3. Hybrid retrieval and optional reranking
4. Evaluation and experiment tracking
5. Error analysis and result visualization
6. Optional grounded answer generation
7. Interactive research dashboard

## Project Structure

```text
ragbench-lab/
├── configs/
│   ├── bm25.yaml
│   ├── dense.yaml
│   └── hybrid.yaml
├── data/
│   └── README.md
├── src/
│   ├── data/
│   ├── retrievers/
│   ├── rerankers/
│   ├── evaluation/
│   └── generation/
├── experiments/
│   ├── run_retrieval.py
│   └── run_ablation.py
├── tests/
├── app/
│   └── streamlit_app.py
├── results/
│   └── README.md
├── reports/
│   └── research_report.md
├── requirements.txt
└── README.md
```

## Getting Started

### Prerequisites

- Python 3.10 or a compatible version supported by the selected dependencies
- Git
- Basic familiarity with Python and machine learning

### Installation

```bash
git clone https://github.com/YOUR_USERNAME/ragbench-lab.git
cd ragbench-lab

python -m venv .venv
```

Activate the virtual environment.

**macOS/Linux**
```bash
source .venv/bin/activate
```

**Windows**
```bash
.venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

The dependency file should be populated with the packages and compatible versions used by the implementation.

### Running Experiments

Once the corresponding scripts are implemented, the intended workflow is:

```bash
python experiments/run_retrieval.py
python experiments/run_ablation.py
```

Launch the dashboard after implementing the application:

```bash
streamlit run app/streamlit_app.py
```

These commands describe the planned interface; they will work once the respective scripts and dependencies have been created.

## Reproducibility

To support reliable comparisons:

- Use the same query set and relevance judgments across retrieval methods.
- Separate development and final evaluation data where appropriate.
- Record model identifiers, configurations, dependency versions, and random seeds.
- Report the evaluation protocol alongside every metric.
- Avoid selecting models using final test results.
- Document hardware, latency measurement conditions, and known limitations.

## Expected Deliverables

- Reproducible implementations of multiple retrieval strategies.
- Quantitative comparisons using standard information retrieval metrics.
- Ablation studies and retrieval failure analysis.
- A research report documenting methodology, findings, and limitations.
- An interactive dashboard for comparing model configurations.
- An optional RAG demonstration that displays retrieved evidence alongside generated answers.

## Limitations

SciFact is a relatively small, domain-specific benchmark. Results on this dataset should not be interpreted as evidence of generalization to all scientific domains or real-world knowledge systems.

Evaluation metrics also have limitations: automated faithfulness scores are imperfect proxies for factual correctness, and latency depends on the hardware and software environment.

## Future Work

- Evaluate additional datasets from the BEIR benchmark.
- Investigate alternative embedding and reranking models.
- Study retrieval robustness under paraphrasing and terminology changes.
- Add uncertainty-aware answer generation and abstention.
- Explore confidence intervals and statistical comparisons across experimental runs.

## References

- BEIR: https://github.com/beir-cellar/beir
- SciFact: https://github.com/allenai/scifact
- Sentence Transformers: https://www.sbert.net/
- Ragas: https://docs.ragas.io/
