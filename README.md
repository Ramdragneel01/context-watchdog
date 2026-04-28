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
- `src/policies.py`: preset policy recipes for common adoption contexts.
- `src/report.py`: rich terminal report renderer.
- `integrations/langchain_wrapper.py`: retriever wrapper.
- `tests/test_watchdog.py`: deterministic pytest suite with mocked models.

## Requirements

- Python 3.11+

Install dependencies:

```bash
pip install -r requirements.txt
```

Install as a package (recommended for production release artifacts):

```bash
pip install .
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

## Policy Recipe Presets

Use one of three built-in policy contexts to adopt quickly:

1. `realtime-support`
2. `regulated-evidence`
3. `offline-batch`

```python
from src.policies import create_policy_watchdog

watchdog, thresholds = create_policy_watchdog("regulated-evidence")
cleaned_chunks, report = watchdog.filter(query, chunks, thresholds=thresholds)
```

Detailed recipe guide: `docs/POLICY-RECIPES.md`.

## Compatibility and Benchmark Evidence

Compatibility matrix and policy benchmark table:

1. `docs/COMPATIBILITY-BENCHMARKS.md`
2. `docs/reports/policy-benchmark-summary.json`

Reproduce benchmark summary:

```bash
python scripts/benchmark_policies.py --iterations 300
```

## Running Tests

```bash
pytest -q
```

## Production Baseline

1. Architecture: `ARCHITECTURE.md`
2. Contribution guide: `CONTRIBUTING.md`
3. Security policy: `SECURITY.md`
4. Changelog: `CHANGELOG.md`
5. Collaboration context: `.claude/CLAUDE.md`
6. API docs: `docs/API.md`
7. Deployment docs: `docs/DEPLOYMENT.md`
8. Testing docs: `docs/TESTING.md`
9. CI workflow: `.github/workflows/ci.yml`
10. Release workflow: `.github/workflows/release.yml`

## Evidence

1. Deterministic unit tests cover relevance, redundancy, conflict, filtering, wrapper behavior, and report output.
2. CI validates test execution and dependency audit checks on `main` and pull requests.
3. Release workflow enforces test pass before GitHub release creation.

## Notes

- If sentence-transformers or transformers models are unavailable, the library falls back to deterministic local heuristics so pipelines can still run.
- Thresholds are tunable to match strictness for different workloads.

## Production Hardening Controls

`ContextWatchdog` supports runtime input bounds to prevent unbounded processing:

1. `max_chunks` (default: `200`)
2. `max_query_chars` (default: `2000`)
3. `max_chunk_chars` (default: `4000`)

Threshold values are validated and must be in the range `[0.0, 1.0]`.

## Limitations

1. Contradiction scoring quality depends on the selected NLI model and available resources.
2. Deterministic fallback embeddings are robust for reliability but less semantically rich than transformer embeddings.
3. This library does not provide built-in persistence or telemetry storage.

## Next Roadmap

1. Add optional batch-processing helpers for high-throughput retrieval pipelines.
2. Add configurable report exporters (JSON and markdown artifact output).
3. Extend integration adapters for additional retriever interfaces.
