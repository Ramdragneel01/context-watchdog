# API Reference

This repository exposes a Python library API for context quality filtering.

## `ContextWatchdog`

Import:

```python
from src.watchdog import ContextWatchdog
```

### Constructor

```python
ContextWatchdog(
    embedding_model_name: str = "all-MiniLM-L6-v2",
    contradiction_model_name: str = "facebook/bart-large-mnli",
    embedding_model: Any | None = None,
    contradiction_classifier: Any | None = None,
    max_chunks: int = 200,
    max_query_chars: int = 2000,
    max_chunk_chars: int = 4000,
)
```

Input bounds:

1. `max_chunks` caps the number of chunks accepted in one call.
2. `max_query_chars` caps query text length after normalization.
3. `max_chunk_chars` caps each chunk length after normalization.

### Methods

1. `score_relevance(query, chunks) -> list[float]`
- Returns per-chunk cosine-based relevance scores in the range `[0, 1]`.

2. `score_redundancy(chunks, similarity_threshold=0.85) -> dict`
- Returns pairwise similarity matrix and redundant pair metadata.

3. `score_conflict(chunks, conflict_threshold=0.70) -> dict`
- Returns contradiction matrix and conflicting pair metadata.

4. `filter(query, chunks, thresholds=None) -> tuple[list[str], dict]`
- Returns cleaned chunks and a structured report with scores and summary.

## `Thresholds`

Import:

```python
from src.watchdog import Thresholds
```

Fields:

1. `relevance_min`
2. `redundancy_max`
3. `conflict_min`

Use `Thresholds.from_mapping(...)` to parse user-provided threshold mappings safely.

Threshold validation:

1. `relevance_min`, `redundancy_max`, and `conflict_min` must be in `[0.0, 1.0]`.
2. Invalid threshold values raise `ValueError`.

## Policy Recipes

Import:

```python
from src.policies import create_policy_watchdog, get_policy_recipe, list_policy_recipes
```

Recipe contexts:

1. `realtime-support`
2. `regulated-evidence`
3. `offline-batch`

Helpers:

1. `list_policy_recipes() -> tuple[str, ...]`
- Lists supported policy names in stable order.

2. `get_policy_recipe(name: str) -> PolicyRecipe`
- Returns thresholds and input limits for one context recipe.

3. `create_policy_watchdog(name: str, threshold_overrides=None) -> tuple[ContextWatchdog, dict]`
- Returns a configured watchdog instance and an explicit thresholds mapping for `filter(...)`.

## `render_report`

Import:

```python
from src.report import render_report
```

Signature:

```python
render_report(report: Mapping[str, Any], console: Console | None = None) -> None
```

Renders summary and per-chunk diagnostics tables using Rich.

## LangChain Wrapper

Import:

```python
from integrations.langchain_wrapper import LangChainWatchdogRetriever
```

Wrapper methods:

1. `get_relevant_documents(query)`
2. `invoke(query)`
3. `aget_relevant_documents(query)`
4. `ainvoke(query)`

The wrapper applies watchdog filtering and stores the latest report in `last_report`.
