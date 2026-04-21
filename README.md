# context-watchdog

Lightweight guardrail library to score and filter RAG context chunks before an LLM call.

## What It Does

- Scores per-chunk relevance against the user query.
- Flags redundant chunks using pairwise similarity.
- Flags conflicting chunks using zero-shot NLI (with deterministic fallback).
- Returns a cleaned chunk list plus a structured diagnostics report.
- Wraps LangChain retrievers as a drop-in quality gate.

## Project Layout

- `src/watchdog.py`: core scoring and filter pipeline.
- `src/report.py`: rich terminal report renderer.
- `integrations/langchain_wrapper.py`: retriever wrapper.
- `tests/test_watchdog.py`: deterministic pytest suite with mocked models.

## Requirements

- Python 3.11+

Install dependencies:

```bash
pip install -r requirements.txt
```

## Quick Start

```python
from src.watchdog import ContextWatchdog
from src.report import render_report

watchdog = ContextWatchdog()
query = "How can I reduce database timeout errors?"
chunks = [
    "Use database indexes to speed up joins.",
    "Use database indexes to speed up joins.",
    "The weather in Madrid is sunny today.",
]

cleaned_chunks, report = watchdog.filter(
    query,
    chunks,
    thresholds={
        "relevance_min": 0.35,
        "redundancy_max": 0.85,
        "conflict_min": 0.70,
    },
)

print(cleaned_chunks)
render_report(report)
```

## LangChain Integration

```python
from integrations.langchain_wrapper import LangChainWatchdogRetriever
from src.watchdog import ContextWatchdog

base_retriever = ...  # your LangChain retriever
wrapped = LangChainWatchdogRetriever(
    retriever=base_retriever,
    watchdog=ContextWatchdog(),
    thresholds={"relevance_min": 0.35, "redundancy_max": 0.85, "conflict_min": 0.70},
)

docs = wrapped.get_relevant_documents("How can I reduce database timeout errors?")
print(wrapped.last_report)
```

## Running Tests

```bash
pytest -q
```

## Notes

- If sentence-transformers or transformers models are unavailable, the library falls back to deterministic local heuristics so pipelines can still run.
- Thresholds are tunable to match strictness for different workloads.
